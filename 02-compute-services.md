# 2. Compute Services 💻

[<- Back: Introduction](./01-introduction.md) | [Next: Storage & Database ->](./03-storage-database.md)

---
- [2a. EC2 Deep Dive](./02a-ec2-deep-dive.md)
- [2b. Serverless Computing](./02b-serverless-computing.md)
- [2c. Container Services](./02c-container-services.md)
---

## Table of Contents

- [Compute Service Categories](#compute-service-categories)
- [Amazon EC2](#amazon-ec2)
- [AWS Lambda](#aws-lambda)
- [Container Services](#container-services)
- [Specialized Compute](#specialized-compute)
- [Choosing the Right Compute Service](#choosing-the-right-compute-service)

## Compute Service Categories

AWS compute services span multiple paradigms, each optimized for different workload characteristics and operational models.

### Traditional Virtual Machines
**EC2** provides resizable compute capacity with full control over the underlying infrastructure, supporting diverse instance types optimized for different workloads.

### Serverless Computing
**Lambda** executes code in response to events without server provisioning, automatically scaling and charging only for actual compute time consumed.

### Container Orchestration
**ECS, EKS, and Fargate** provide managed container platforms supporting Docker and Kubernetes ecosystems with varying levels of infrastructure abstraction.

### Specialized Compute
**Batch, Lightsail, and Outposts** address specific use cases like high-performance computing, simplified deployments, and hybrid cloud scenarios.

## Amazon EC2

Elastic Compute Cloud forms the foundation of AWS compute, offering virtual servers with comprehensive customization capabilities.

### Instance Types and Families

AWS organizes EC2 instances into families optimized for specific workload patterns.

**General Purpose (M, T, A)**
- Balanced CPU, memory, and networking
- T-series: Burstable performance for variable workloads
- M-series: Consistent performance for diverse applications
- A-series: ARM-based processors for cost optimization

**Compute Optimized (C)**
- High-performance processors for CPU-intensive tasks
- Scientific computing, web servers, gaming
- Up to 3.9 GHz sustained all-core turbo frequency

**Memory Optimized (R, X, Z)**
- High memory-to-CPU ratios for memory-intensive applications
- R-series: General memory optimization
- X-series: Extreme memory capacity (up to 4TB RAM)
- Z-series: High-frequency processors with large memory

**Storage Optimized (I, D, H)**
- High sequential read/write access to local storage
- I-series: NVMe SSD storage for databases
- D-series: Dense HDD storage for distributed computing
- H-series: High disk throughput for big data workloads

**Accelerated Computing (P, G, F)**
- Hardware accelerators for machine learning and graphics
- P-series: GPU instances for AI/ML training and inference
- G-series: GPU instances for graphics workstations
- F-series: FPGA instances for hardware acceleration

### Instance Purchasing Options

**On-Demand Instances**
```
Characteristics:
- Pay per hour/second with no commitment
- Highest flexibility, highest cost
- Immediate availability
- Ideal for: Development, testing, unpredictable workloads
```

**Reserved Instances**
```
Commitment Levels:
- 1-year terms: 20-40% savings
- 3-year terms: 40-60% savings

Payment Options:
- No Upfront: Lower discount, pay monthly
- Partial Upfront: Moderate discount, split payment
- All Upfront: Maximum discount, single payment
```

**Spot Instances**
```
Market-Based Pricing:
- Up to 90% savings over On-Demand
- 2-minute interruption warning
- Price fluctuates based on supply/demand

Best Practices:
- Implement graceful shutdown handling
- Use Spot Fleet for diversification
- Combine with On-Demand for baseline capacity
```

### EC2 Networking

**Elastic Network Interfaces (ENI)**
- Virtual network cards attachable to instances
- Support multiple private IPs and security groups
- Enable network redundancy and hot-swapping

**Enhanced Networking**
- Single Root I/O Virtualization (SR-IOV)
- Higher bandwidth and lower latency
- Required for cluster compute instances

**Placement Groups**
- **Cluster**: Low latency within single AZ
- **Partition**: Spread across hardware partitions
- **Spread**: Each instance on separate hardware

### Storage Options

**Instance Store**
```
Characteristics:
- Temporary storage physically attached to host
- High IOPS and throughput
- Data lost on instance stop/termination
- Included in instance pricing
```

**Amazon EBS**
```
Volume Types:
- gp3: General purpose SSD with configurable IOPS
- io2: Provisioned IOPS SSD for mission-critical
- st1: Throughput optimized HDD for big data
- sc1: Cold HDD for infrequent access
```

## AWS Lambda

Serverless computing service enabling event-driven code execution without infrastructure management.

### Core Concepts

**Function as Unit of Deployment**
- Single-purpose code packages with dependencies
- Runtime environment selection (Node.js, Python, Java, etc.)
- Memory allocation determines CPU and networking performance

**Event-Driven Execution**
- Synchronous invocation with immediate response
- Asynchronous invocation with automatic retry
- Stream-based invocation for real-time processing

**Automatic Scaling**
```
Scaling Behavior:
- Concurrent executions scale automatically
- Default limit: 1000 concurrent executions
- Cold start latency for new execution environments
- Warm execution contexts reused when possible
```

### Lambda Programming Model

**Handler Function**
```python
import json

def lambda_handler(event, context):
    # Process the event
    result = process_data(event['data'])
    
    # Return response
    return {
        'statusCode': 200,
        'body': json.dumps(result)
    }
```

**Context Object**
```python
# Available context properties
context.function_name        # Function name
context.function_version     # Function version
context.memory_limit_in_mb   # Memory allocation
context.remaining_time_in_millis  # Execution time remaining
```

### Performance Optimization

**Memory Configuration**
- Memory range: 128MB to 10,240MB (10GB)
- CPU allocation scales proportionally with memory
- Network bandwidth increases with memory allocation

**Cold Start Mitigation**
```
Strategies:
- Provisioned Concurrency for predictable latency
- Connection pooling and global variable reuse
- Minimize deployment package size
- Use runtime-specific optimization techniques
```

**Execution Environment Lifecycle**
```
Phases:
1. Init: Download code and initialize runtime
2. Invoke: Execute handler function
3. Shutdown: Clean up execution environment

Optimization:
- Initialize expensive resources outside handler
- Reuse connections and clients across invocations
- Cache configuration and authentication tokens
```

## Container Services

AWS provides multiple container orchestration options supporting different operational models and Kubernetes compatibility.

### Amazon ECS (Elastic Container Service)

**AWS-Native Container Orchestration**
- Fully managed container orchestration service
- Deep integration with AWS services and IAM
- Support for both EC2 and Fargate launch types

**Task Definitions**
```json
{
  "family": "web-server",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "nginx:latest",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp"
        }
      ]
    }
  ]
}
```

**Service Management**
- Desired count maintenance and auto-scaling
- Rolling updates with configurable deployment strategies
- Load balancer integration and service discovery

### Amazon EKS (Elastic Kubernetes Service)

**Managed Kubernetes Control Plane**
- Certified Kubernetes distribution with upstream compatibility
- Automated cluster provisioning and management
- Integration with AWS services through IAM and VPC

**Node Group Management**
```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: production-cluster
  region: us-west-2

nodeGroups:
  - name: worker-nodes
    instanceType: m5.large
    desiredCapacity: 3
    minSize: 1
    maxSize: 10
    volumeSize: 20
```

**Add-ons and Integrations**
- AWS Load Balancer Controller for ingress
- EBS CSI driver for persistent storage
- VPC CNI for pod networking
- AWS Secrets Manager integration

### AWS Fargate

**Serverless Container Execution**
- Infrastructure abstraction for containers
- Pay-per-use pricing based on vCPU and memory
- Automatic scaling without capacity planning

**Resource Specifications**
```
Supported Configurations:
CPU (vCPU):    0.25, 0.5, 1, 2, 4, 8, 16
Memory (GB):   0.5-120 (varies by CPU)

Pricing Model:
- Per-second billing
- Separate charges for CPU and memory
- No data transfer charges within AZ
```

## Specialized Compute

### AWS Batch

**Managed Batch Computing**
- Job queues and compute environments
- Automatic scaling based on job requirements
- Support for EC2 and Fargate compute environments

### Amazon Lightsail

**Simplified Virtual Private Servers**
- Predictable monthly pricing
- Pre-configured application stacks
- Integrated networking and storage

### AWS Outposts

**On-Premises AWS Infrastructure**
- AWS services running on customer premises
- Consistent hybrid cloud experience
- Local data processing and storage

## Choosing the Right Compute Service

Decision matrix for selecting appropriate compute services based on workload characteristics.

### Workload Analysis Framework

| Characteristic | EC2 | Lambda | ECS/EKS | Fargate |
|---------------|-----|---------|---------|---------|
| **Duration** | Any | < 15 min | Any | Any |
| **Predictability** | Variable | Event-driven | Scheduled/Continuous | Variable |
| **Scaling** | Manual/Auto | Automatic | Orchestrated | Automatic |
| **State** | Stateful | Stateless | Both | Stateless |
| **Control** | Full | Limited | Medium | Limited |

### Use Case Patterns

**EC2 Optimal Scenarios**
- Legacy application migration
- Licensed software requiring specific instances
- High-performance computing workloads
- Long-running background processes

**Lambda Optimal Scenarios**
- Event processing and data transformation
- API backends with variable traffic
- Scheduled tasks and automation
- Real-time file and data processing

**Container Service Scenarios**
- Microservices architectures
- Application modernization
- CI/CD pipeline components
- Multi-environment consistency

### Cost Optimization Strategies

**Right-Sizing Approach**
1. Monitor resource utilization patterns
2. Analyze performance requirements vs. allocated resources
3. Implement automated scaling policies
4. Regular review and adjustment cycles

**Multi-Service Architecture**
```
Pattern Examples:
- Lambda for API gateway and event processing
- ECS for long-running application services
- EC2 for databases and stateful components
- Spot instances for batch processing workloads
```

---

[<- Back: Introduction](./01-introduction.md) | [Next: Storage & Database ->](./03-storage-database.md)
