# 2a. EC2 Deep Dive 🖥️

[<- Back to Compute Services](./02-compute-services.md) | [Next Sub-Topic: Serverless Computing ->](./02b-serverless-computing.md)

## Overview

This sub-topic provides detailed exploration of Amazon EC2 beyond the basics, covering advanced configurations, optimization strategies, and real-world implementation patterns. Understanding EC2 at this level enables architectural decisions that balance performance, cost, and operational complexity.

## Key Concepts

### Instance Lifecycle Management

EC2 instances progress through defined states with specific characteristics and billing implications.

**State Transitions**
```
pending → running → stopping → stopped
                 ↘ shutting-down → terminated
                 ↘ rebooting → running
```

**Hibernate Capability**
- Preserves in-memory state to EBS root volume
- Faster startup compared to cold boot
- Instance RAM must be less than 150GB
- Supported on M3, M4, M5, R3, R4, R5 instances

### Advanced Networking

**Enhanced Networking (SR-IOV)**
```bash
# Enable enhanced networking on instance
aws ec2 modify-instance-attribute \\
    --instance-id i-1234567890abcdef0 \\
    --ena-support
```

**Elastic Fabric Adapter (EFA)**
- Specialized network interface for HPC workloads
- Bypass kernel for low-latency inter-node communication
- Support for MPI and NCCL communication libraries

## Implementation Patterns

### Pattern 1: Auto Scaling with Mixed Instance Types

Optimize cost and availability by diversifying instance types within Auto Scaling groups.

```json
{
  \"AutoScalingGroupName\": \"mixed-instance-asg\",
  \"MixedInstancesPolicy\": {
    \"LaunchTemplate\": {
      \"LaunchTemplateSpecification\": {
        \"LaunchTemplateName\": \"web-server-template\",
        \"Version\": \"$Latest\"
      },
      \"Overrides\": [
        {\"InstanceType\": \"m5.large\", \"AvailabilityZone\": \"us-west-2a\"},
        {\"InstanceType\": \"m5a.large\", \"AvailabilityZone\": \"us-west-2b\"},
        {\"InstanceType\": \"m4.large\", \"AvailabilityZone\": \"us-west-2c\"}
      ]
    },
    \"InstancesDistribution\": {
      \"OnDemandPercentage\": 20,
      \"SpotAllocationStrategy\": \"diversified\"
    }
  }
}
```

**When to use this pattern:**
- Cost optimization is primary concern
- Application tolerates instance interruptions
- Workload can run across multiple instance types

### Pattern 2: Dedicated Hosts for License Compliance

Maintain software license compliance while utilizing cloud elasticity.

```bash
# Allocate dedicated host
aws ec2 allocate-hosts \\
    --instance-type m5.large \\
    --quantity 1 \\
    --availability-zone us-west-2a

# Launch instance on dedicated host
aws ec2 run-instances \\
    --image-id ami-12345678 \\
    --instance-type m5.large \\
    --placement Tenancy=host,HostId=h-0123456789abcdef0
```

**When to use this pattern:**
- Bring-your-own-license (BYOL) software requirements
- Regulatory compliance requiring physical isolation
- Legacy applications with specific hardware dependencies

## Common Challenges and Solutions

### Challenge 1: Instance Right-Sizing

Balancing performance requirements with cost optimization across diverse workloads.

**Solution:**

```python
import boto3
import json
from datetime import datetime, timedelta

def analyze_instance_utilization(instance_id, days=30):
    \"\"\"
    Analyze instance utilization patterns for right-sizing recommendations
    \"\"\"
    cloudwatch = boto3.client('cloudwatch')
    ec2 = boto3.client('ec2')
    
    # Get instance details
    instance = ec2.describe_instances(InstanceIds=[instance_id])
    instance_type = instance['Reservations'][0]['Instances'][0]['InstanceType']
    
    # Define metrics to analyze
    metrics = ['CPUUtilization', 'NetworkIn', 'NetworkOut']
    end_time = datetime.utcnow()
    start_time = end_time - timedelta(days=days)
    
    utilization_data = {}
    
    for metric in metrics:
        response = cloudwatch.get_metric_statistics(
            Namespace='AWS/EC2',
            MetricName=metric,
            Dimensions=[{'Name': 'InstanceId', 'Value': instance_id}],
            StartTime=start_time,
            EndTime=end_time,
            Period=3600,  # 1 hour intervals
            Statistics=['Average', 'Maximum']
        )
        
        if response['Datapoints']:
            utilization_data[metric] = {
                'avg': sum(point['Average'] for point in response['Datapoints']) / len(response['Datapoints']),
                'max': max(point['Maximum'] for point in response['Datapoints'])
            }
    
    # Generate right-sizing recommendation
    cpu_avg = utilization_data.get('CPUUtilization', {}).get('avg', 0)
    
    if cpu_avg < 20:
        recommendation = \"Consider downsizing - low CPU utilization\"
    elif cpu_avg > 80:
        recommendation = \"Consider upsizing - high CPU utilization\"
    else:
        recommendation = \"Instance appears properly sized\"
    
    return {
        'instance_id': instance_id,
        'current_type': instance_type,
        'utilization': utilization_data,
        'recommendation': recommendation
    }
```

### Challenge 2: Spot Instance Interruption Handling

Managing workload continuity when spot instances are reclaimed by AWS.

**Solution:**

```bash
#!/bin/bash
# Spot instance interruption handler script

# Monitor for spot interruption notice
while true; do
    INTERRUPT_NOTICE=$(curl -s http://169.254.169.254/latest/meta-data/spot/instance-action 2>/dev/null)
    
    if [ \"$INTERRUPT_NOTICE\" != \"\" ]; then
        echo \"Spot interruption notice received: $INTERRUPT_NOTICE\"
        
        # Graceful shutdown sequence
        echo \"Starting graceful shutdown...\"
        
        # 1. Stop accepting new work
        systemctl stop application-service
        
        # 2. Complete current tasks (with timeout)
        timeout 90s /opt/app/drain-tasks.sh
        
        # 3. Save state if necessary
        /opt/app/save-checkpoint.sh
        
        # 4. Deregister from load balancer
        aws elbv2 deregister-targets \\
            --target-group-arn arn:aws:elasticloadbalancing:region:account:targetgroup/my-targets \\
            --targets Id=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
        
        echo \"Graceful shutdown complete\"
        break
    fi
    
    sleep 5
done
```

## Practical Example

Complete implementation of a fault-tolerant web application using EC2 Auto Scaling with mixed instance types and spot instances.

```python
import boto3
import json

class FaultTolerantWebApp:
    def __init__(self, region='us-west-2'):
        self.ec2 = boto3.client('ec2', region_name=region)
        self.autoscaling = boto3.client('autoscaling', region_name=region)
        self.elbv2 = boto3.client('elbv2', region_name=region)
        
    def create_launch_template(self):
        \"\"\"Create launch template with user data for application setup\"\"\"
        user_data = '''#!/bin/bash
        yum update -y
        yum install -y docker
        systemctl start docker
        systemctl enable docker
        
        # Install spot interruption handler
        cat > /opt/spot-handler.sh << 'EOF'
        #!/bin/bash
        # Spot interruption monitoring script from above
        EOF
        chmod +x /opt/spot-handler.sh
        nohup /opt/spot-handler.sh &
        
        # Start application container
        docker run -d -p 80:8080 --name webapp myapp:latest
        '''
        
        response = self.ec2.create_launch_template(
            LaunchTemplateName='fault-tolerant-webapp',
            LaunchTemplateData={
                'ImageId': 'ami-0c55b159cbfafe1d0',  # Amazon Linux 2
                'InstanceType': 'm5.large',
                'SecurityGroupIds': ['sg-12345678'],
                'UserData': user_data,
                'IamInstanceProfile': {
                    'Name': 'EC2InstanceProfile'
                },
                'Monitoring': {'Enabled': True},
                'TagSpecifications': [{
                    'ResourceType': 'instance',
                    'Tags': [
                        {'Key': 'Name', 'Value': 'webapp-instance'},
                        {'Key': 'Environment', 'Value': 'production'}
                    ]
                }]
            }
        )
        return response['LaunchTemplate']['LaunchTemplateId']
    
    def create_auto_scaling_group(self, template_id, target_group_arn):
        \"\"\"Create Auto Scaling group with mixed instance types\"\"\"
        self.autoscaling.create_auto_scaling_group(
            AutoScalingGroupName='webapp-asg',
            MinSize=2,
            MaxSize=10,
            DesiredCapacity=4,
            TargetGroupARNs=[target_group_arn],
            VPCZoneIdentifier='subnet-12345,subnet-67890,subnet-abcde',
            MixedInstancesPolicy={
                'LaunchTemplate': {
                    'LaunchTemplateSpecification': {
                        'LaunchTemplateId': template_id,
                        'Version': '$Latest'
                    },
                    'Overrides': [
                        {'InstanceType': 'm5.large'},
                        {'InstanceType': 'm5a.large'},
                        {'InstanceType': 'm4.large'},
                        {'InstanceType': 'c5.large'}
                    ]
                },
                'InstancesDistribution': {
                    'OnDemandBaseCapacity': 1,
                    'OnDemandPercentageAboveBaseCapacity': 25,
                    'SpotAllocationStrategy': 'diversified',
                    'SpotMaxPrice': '0.10'
                }
            }
        )
        
        # Create CPU-based scaling policy
        self.autoscaling.put_scaling_policy(
            AutoScalingGroupName='webapp-asg',
            PolicyName='cpu-target-tracking',
            PolicyType='TargetTrackingScaling',
            TargetTrackingConfiguration={
                'PredefinedMetricSpecification': {
                    'PredefinedMetricType': 'ASGAverageCPUUtilization'
                },
                'TargetValue': 70.0,
                'ScaleOutCooldown': 300,
                'ScaleInCooldown': 300
            }
        )
    
    def deploy_complete_stack(self):
        \"\"\"Deploy the complete fault-tolerant web application\"\"\"
        # Create launch template
        template_id = self.create_launch_template()
        
        # Create target group (assuming ALB exists)
        target_group_response = self.elbv2.create_target_group(
            Name='webapp-targets',
            Protocol='HTTP',
            Port=80,
            VpcId='vpc-12345678',
            HealthCheckProtocol='HTTP',
            HealthCheckPath='/health',
            HealthCheckIntervalSeconds=30,
            HealthyThresholdCount=2,
            UnhealthyThresholdCount=3
        )
        
        target_group_arn = target_group_response['TargetGroups'][0]['TargetGroupArn']
        
        # Create Auto Scaling group
        self.create_auto_scaling_group(template_id, target_group_arn)
        
        return {
            'launch_template_id': template_id,
            'target_group_arn': target_group_arn,
            'auto_scaling_group': 'webapp-asg'
        }

# Usage example
app = FaultTolerantWebApp()
deployment = app.deploy_complete_stack()
print(f\"Deployed fault-tolerant web app: {deployment}\")
```

## Summary

Key points covered in this EC2 deep dive:

1. **Advanced instance lifecycle management** including hibernation and state transitions
2. **Enhanced networking capabilities** with SR-IOV and EFA for high-performance workloads
3. **Cost optimization patterns** using mixed instance types and spot instances
4. **Fault tolerance strategies** including graceful spot instance interruption handling
5. **Comprehensive automation** for deploying resilient, cost-effective EC2 architectures

## Next Steps

The next sub-topic will explore serverless computing with AWS Lambda, demonstrating how to combine EC2-based infrastructure with event-driven serverless components for hybrid architectures that optimize both cost and operational complexity.

---

[<- Back to Compute Services](./02-compute-services.md) | [Next Sub-Topic: Serverless Computing ->](./02b-serverless-computing.md)
