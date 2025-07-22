# 1. Introduction to AWS ☁️

[<- Back: AWS Glossary](./00-glossary-aws-services.md) | [Next: Compute Services ->](./02-compute-services.md)

## Table of Contents

- [What is AWS?](#what-is-aws)
- [Cloud Computing Models](#cloud-computing-models)
- [AWS Global Infrastructure](#aws-global-infrastructure)
- [Core Concepts](#core-concepts)
- [AWS Pricing Models](#aws-pricing-models)
- [Getting Started](#getting-started)

## What is AWS?

Amazon Web Services (AWS) is a comprehensive cloud computing platform that provides on-demand computing resources and services over the internet. Launched in 2006, AWS has become the world's most comprehensive and broadly adopted cloud platform.

### Key Value Propositions

**Scalability** - Resources scale elastically based on demand, from single instances to global deployments.

**Cost Efficiency** - Pay-as-you-go pricing eliminates upfront capital expenses and reduces operational overhead.

**Reliability** - Built-in redundancy and fault tolerance across geographically distributed infrastructure.

**Security** - Industry-leading security practices with shared responsibility model and compliance certifications.

**Innovation Velocity** - Rapid access to cutting-edge technologies without infrastructure investment.

## Cloud Computing Models

AWS operates across multiple service models, each providing different levels of abstraction and control.

### Infrastructure as a Service (IaaS)

**Definition**: Fundamental computing resources delivered over the internet.

*AWS Examples:*
- EC2 (Virtual servers)
- EBS (Block storage)
- VPC (Networking)

*Use Cases:*
- Legacy application migration
- Development/testing environments
- Backup and disaster recovery

### Platform as a Service (PaaS)

**Definition**: Application deployment and management platforms without infrastructure complexity.

*AWS Examples:*
- Elastic Beanstalk (Application platform)
- Lambda (Serverless functions)
- RDS (Managed databases)

*Use Cases:*
- Rapid application development
- Microservices architectures
- API development

### Software as a Service (SaaS)

**Definition**: Complete applications delivered over the internet.

*AWS Examples:*
- WorkSpaces (Virtual desktops)
- Chime (Communications)
- QuickSight (Business intelligence)

*Use Cases:*
- End-user productivity applications
- Business analytics
- Collaboration tools

## AWS Global Infrastructure

AWS infrastructure spans globally across multiple geographic regions, providing low latency and high availability.

### Regions

**Geographic Areas** containing multiple isolated locations called Availability Zones.

*Key Characteristics:*
- Currently 33 regions worldwide
- Each region completely independent
- Data sovereignty and compliance boundaries
- Natural disaster isolation

*Selection Criteria:*
- Latency to end users
- Data sovereignty requirements
- Service availability
- Cost considerations

### Availability Zones (AZs)

**Isolated Data Centers** within regions, connected by high-bandwidth, low-latency networking.

*Key Characteristics:*
- Minimum 3 AZs per region
- Physical and logical separation
- Independent power, cooling, networking
- Sub-millisecond latency between AZs

*Design Principle:*
Applications spanning multiple AZs achieve higher availability than single-AZ deployments.

### Edge Locations

**Global Content Distribution** points for CloudFront CDN and other edge services.

*Key Characteristics:*
- 400+ edge locations globally
- Cache content closer to users
- Reduce latency and improve performance
- Support for edge computing workloads

## Core Concepts

Understanding fundamental AWS concepts is essential for effective cloud architecture.

### Shared Responsibility Model

AWS and customers share security and compliance responsibilities across different layers.

| Layer | AWS Responsibility | Customer Responsibility |
|-------|-------------------|------------------------|
| **Infrastructure** | Physical security, host OS patching | - |
| **Platform** | Managed service configuration | Service configuration, access controls |
| **Application** | - | Code, data, identity management |
| **Data** | - | Encryption, backup, access patterns |

### Well-Architected Framework

AWS provides architectural principles organized into six pillars for building robust cloud solutions.

**Operational Excellence** - Run and monitor systems to deliver business value and improve processes.

**Security** - Protect information, systems, and assets while delivering business value.

**Reliability** - Ensure workloads perform intended functions correctly and consistently.

**Performance Efficiency** - Use computing resources efficiently to meet requirements and maintain efficiency as demand changes.

**Cost Optimization** - Run systems to deliver business value at the lowest price point.

**Sustainability** - Minimize environmental impact of running cloud workloads.

### Identity and Access Management (IAM)

Central service for managing access to AWS resources through users, groups, roles, and policies.

**Users** - Individual identities with permanent credentials.

**Groups** - Collections of users with shared permissions.

**Roles** - Temporary access credentials for services or federated access.

**Policies** - JSON documents defining permissions using effect, action, resource, and condition elements.

## AWS Pricing Models

AWS offers multiple pricing models to optimize costs based on usage patterns and commitment levels.

### On-Demand Pricing

**Pay-as-you-go** model with no upfront costs or long-term commitments.

*Characteristics:*
- Highest per-hour cost
- Maximum flexibility
- No capacity planning required
- Ideal for unpredictable workloads

### Reserved Instances

**Capacity reservations** with significant discounts for 1-3 year commitments.

*Types:*
- **Standard Reserved**: Up to 75% savings, fixed instance attributes
- **Convertible Reserved**: Up to 54% savings, changeable instance attributes
- **Scheduled Reserved**: Capacity reservations for recurring time windows

### Spot Instances

**Unused EC2 capacity** available at up to 90% discount with interruption risk.

*Characteristics:*
- Market-driven pricing
- 2-minute interruption notice
- Ideal for fault-tolerant, flexible workloads
- Batch processing, data analysis use cases

### Savings Plans

**Flexible pricing** model offering savings in exchange for consistent usage commitment.

*Types:*
- **Compute Savings Plans**: Up to 66% savings across EC2, Lambda, Fargate
- **EC2 Instance Savings Plans**: Up to 72% savings for specific instance families

### Free Tier

**12-month trial** for new accounts with limited usage of core services.

*Examples:*
- 750 hours/month EC2 t2.micro instances
- 5GB S3 storage
- 1 million Lambda requests
- 25GB DynamoDB storage

## Getting Started

Essential steps for beginning your AWS journey effectively.

### Account Setup

1. **Create AWS Account** with valid payment method and contact information
2. **Enable MFA** on root account for security
3. **Create IAM User** for day-to-day operations instead of using root
4. **Set up Billing Alerts** to monitor spending

### Essential First Services

**AWS CLI** - Command-line interface for programmatic access to AWS services.

**CloudShell** - Browser-based shell environment with pre-configured AWS CLI.

**AWS Console** - Web-based interface for managing AWS resources.

**CloudFormation** - Infrastructure as Code service for reproducible deployments.

### Learning Resources

**AWS Training and Certification** - Structured learning paths for different roles and expertise levels.

**AWS Documentation** - Comprehensive technical documentation for all services.

**AWS Well-Architected Labs** - Hands-on exercises implementing best practices.

**AWS Samples** - Code examples and reference architectures on GitHub.

### Cost Management

**AWS Cost Explorer** - Analyze spending patterns and forecast future costs.

**AWS Budgets** - Set custom budgets with alerts for spending thresholds.

**AWS Trusted Advisor** - Recommendations for cost optimization, security, and performance.

**Resource Tagging** - Organize and track costs by project, environment, or department.

---

[<- Back: AWS Glossary](./00-glossary-aws-services.md) | [Next: Compute Services ->](./02-compute-services.md)
