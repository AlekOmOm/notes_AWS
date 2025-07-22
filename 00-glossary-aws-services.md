# 00. AWS Services Glossary 📖

[<- Back: Main Note](./README.md) | [Next: Introduction ->](./01-introduction.md)

## Table of Contents

- [Compute Services](#compute-services)
- [Storage Services](#storage-services)
- [Database Services](#database-services)
- [Networking & Content Delivery](#networking--content-delivery)
- [Security, Identity & Compliance](#security-identity--compliance)
- [Machine Learning](#machine-learning)
- [Analytics](#analytics)
- [Developer Tools](#developer-tools)
- [Management & Governance](#management--governance)
- [Internet of Things (IoT)](#internet-of-things-iot)
- [Application Integration](#application-integration)

## Compute Services

Core services for running applications and workloads.

### Amazon EC2 (Elastic Compute Cloud)
**Virtual servers in the cloud** - Scalable compute capacity with configurable instances for diverse workloads.

*Key characteristics:*
- Resizable compute capacity
- Multiple instance types and sizes
- Pay-as-you-go pricing
- Integration with other AWS services

### AWS Lambda
**Serverless computing** - Run code without provisioning or managing servers, paying only for compute time consumed.

*Key characteristics:*
- Event-driven execution
- Automatic scaling
- Sub-second billing
- Supports multiple runtime environments

### Amazon ECS (Elastic Container Service)
**Container orchestration** - Fully managed service for running Docker containers at scale.

*Key characteristics:*
- Docker container support
- Service discovery
- Load balancing integration
- Auto scaling capabilities

### Amazon EKS (Elastic Kubernetes Service)
**Managed Kubernetes** - Run Kubernetes applications without cluster management overhead.

*Key characteristics:*
- Kubernetes API compatibility
- Automatic updates and patching
- Integration with AWS services
- Multi-AZ deployment

### AWS Fargate
**Serverless containers** - Run containers without managing the underlying infrastructure.

*Key characteristics:*
- Infrastructure abstraction
- Per-second billing
- Automatic scaling
- Security isolation

## Storage Services

Data storage solutions for different use cases and access patterns.

### Amazon S3 (Simple Storage Service)
**Object storage** - Scalable storage for any amount of data with high durability and availability.

*Key characteristics:*
- 99.999999999% (11 9's) durability
- Multiple storage classes
- Lifecycle management
- Versioning and encryption

### Amazon EBS (Elastic Block Store)
**Block storage** - High-performance block storage for EC2 instances with different volume types.

*Key characteristics:*
- Persistent storage
- Snapshot capabilities
- Encryption support
- Multiple volume types (gp3, io2, etc.)

### Amazon EFS (Elastic File System)
**Managed file storage** - Scalable, elastic file system for use with AWS services and on-premises resources.

*Key characteristics:*
- POSIX-compliant
- Automatic scaling
- Multi-AZ access
- Performance modes

### Amazon Glacier
**Archival storage** - Low-cost storage for data archiving and long-term backup with flexible retrieval options.

*Key characteristics:*
- Extremely low cost
- Multiple retrieval options
- Vault lock for compliance
- Integration with S3 lifecycle

## Database Services

Managed database solutions for relational and NoSQL workloads.

### Amazon RDS (Relational Database Service)
**Managed relational databases** - Automated database administration for popular engines.

*Key characteristics:*
- Multiple engine support (MySQL, PostgreSQL, Oracle, SQL Server)
- Automated backups and patching
- Multi-AZ deployment
- Read replicas

### Amazon DynamoDB
**NoSQL database** - Fully managed, serverless NoSQL database with single-digit millisecond performance.

*Key characteristics:*
- Serverless architecture
- Automatic scaling
- Global tables
- Built-in security features

### Amazon Redshift
**Data warehouse** - Fully managed data warehouse for analytics workloads.

*Key characteristics:*
- Columnar storage
- Massively parallel processing
- SQL compatibility
- Data lake integration

### Amazon Aurora
**Cloud-native relational database** - MySQL and PostgreSQL-compatible database with cloud-optimized performance.

*Key characteristics:*
- Up to 5x MySQL performance
- Up to 3x PostgreSQL performance
- Automatic failover
- Serverless option available

## Networking & Content Delivery

Services for connecting and delivering content globally.

### Amazon VPC (Virtual Private Cloud)
**Isolated cloud resources** - Logically isolated virtual network within AWS cloud.

*Key characteristics:*
- Complete network control
- Subnet creation and management
- Security group configuration
- Internet and VPN connectivity

### Amazon Route 53
**DNS service** - Scalable domain name system and domain registration service.

*Key characteristics:*
- Global DNS resolution
- Health checking
- Traffic routing policies
- Domain registration

### Amazon CloudFront
**Content delivery network** - Global CDN for delivering content with low latency.

*Key characteristics:*
- Global edge locations
- Dynamic and static content delivery
- SSL/TLS encryption
- Real-time metrics

### AWS Direct Connect
**Dedicated network connection** - Establish dedicated connectivity between on-premises and AWS.

*Key characteristics:*
- Consistent network performance
- Reduced bandwidth costs
- Private connectivity
- Virtual interface support

## Security, Identity & Compliance

Services for securing AWS resources and managing access.

### AWS IAM (Identity and Access Management)
**Access control** - Manage users, groups, roles, and permissions for AWS resources.

*Key characteristics:*
- Fine-grained permissions
- Multi-factor authentication
- Cross-account access
- Policy-based management

### AWS KMS (Key Management Service)
**Encryption key management** - Create and control encryption keys for data protection.

*Key characteristics:*
- Hardware security modules
- Key rotation
- Audit logging
- Integration with AWS services

### AWS Shield
**DDoS protection** - Managed protection against distributed denial of service attacks.

*Key characteristics:*
- Always-on detection
- Automatic mitigation
- Advanced threat intelligence
- 24/7 support (Shield Advanced)

### AWS WAF (Web Application Firewall)
**Application protection** - Filter malicious web traffic and protect applications from common exploits.

*Key characteristics:*
- Customizable rules
- Real-time monitoring
- Integration with CloudFront and ALB
- Managed rule sets

## Machine Learning

AI and machine learning services for intelligent applications.

### Amazon SageMaker
**ML platform** - Complete platform for building, training, and deploying machine learning models.

*Key characteristics:*
- Jupyter notebook instances
- Built-in algorithms
- Automatic model tuning
- Managed endpoints

### AWS Rekognition
**Image and video analysis** - Deep learning-powered image and video analysis service.

*Key characteristics:*
- Object and scene detection
- Facial analysis and recognition
- Content moderation
- Celebrity recognition

### Amazon Comprehend
**Natural language processing** - Extract insights and relationships from text using machine learning.

*Key characteristics:*
- Sentiment analysis
- Entity recognition
- Key phrase extraction
- Language detection

### Amazon Lex
**Conversational AI** - Build chatbots and voice applications with natural language understanding.

*Key characteristics:*
- Speech recognition
- Natural language understanding
- Intent recognition
- Integration with AWS services

## Analytics

Services for data analysis and business intelligence.

### Amazon Athena
**Interactive query service** - Analyze data in S3 using standard SQL without infrastructure management.

*Key characteristics:*
- Serverless architecture
- Standard SQL queries
- Pay-per-query pricing
- Integration with AWS Glue

### Amazon EMR (Elastic MapReduce)
**Big data processing** - Managed cluster platform for processing vast amounts of data.

*Key characteristics:*
- Apache Hadoop and Spark support
- Auto scaling
- Spot instance integration
- Multiple data processing frameworks

### Amazon Kinesis
**Real-time data streaming** - Process and analyze streaming data in real-time.

*Key characteristics:*
- Real-time processing
- Scalable data streams
- Multiple processing options
- Integration with analytics tools

### AWS Glue
**ETL service** - Serverless data integration service for discovering, preparing, and combining data.

*Key characteristics:*
- Automatic schema discovery
- Visual ETL job creation
- Serverless execution
- Data catalog management

## Developer Tools

Services for application development and deployment.

### AWS CodeCommit
**Source control** - Fully managed Git-based source control service.

*Key characteristics:*
- Private Git repositories
- High availability
- Encryption in transit and at rest
- IAM integration

### AWS CodeBuild
**Continuous integration** - Managed build service that compiles source code and runs tests.

*Key characteristics:*
- Fully managed build environment
- Pre-configured build environments
- Custom build environments
- Pay-as-you-go pricing

### AWS CodeDeploy
**Application deployment** - Automate code deployments to various compute services.

*Key characteristics:*
- Automated deployments
- Blue/green deployments
- Rolling deployments
- Rollback capabilities

### AWS CodePipeline
**Continuous delivery** - Build, test, and deploy applications with continuous delivery workflows.

*Key characteristics:*
- Visual workflow
- Integration with third-party tools
- Parallel execution
- Approval actions

## Management & Governance

Services for monitoring, managing, and governing AWS resources.

### AWS CloudWatch
**Monitoring and observability** - Monitor applications, infrastructure, and services with metrics and logs.

*Key characteristics:*
- Real-time monitoring
- Custom metrics
- Alarms and notifications
- Log aggregation and analysis

### AWS CloudFormation
**Infrastructure as code** - Model and provision AWS resources using templates.

*Key characteristics:*
- JSON/YAML templates
- Stack management
- Change sets
- Drift detection

### AWS Config
**Configuration management** - Track resource configurations and compliance.

*Key characteristics:*
- Configuration history
- Compliance rules
- Resource relationships
- Remediation actions

### AWS Systems Manager
**Operational management** - Unified interface for managing AWS resources and on-premises servers.

*Key characteristics:*
- Parameter store
- Patch management
- Session manager
- Automation workflows

## Internet of Things (IoT)

Services for connecting and managing IoT devices.

### AWS IoT Core
**IoT connectivity** - Connect billions of IoT devices to the cloud securely.

*Key characteristics:*
- Device connectivity
- Message routing
- Device shadows
- Rules engine

### AWS IoT Greengrass
**Edge computing** - Run local compute, messaging, and machine learning inference on connected devices.

*Key characteristics:*
- Local processing
- Offline operation
- ML inference at edge
- Secure communication

## Application Integration

Services for decoupling and integrating applications.

### Amazon SNS (Simple Notification Service)
**Pub/Sub messaging** - Fully managed messaging service for application-to-application communication.

*Key characteristics:*
- Topic-based messaging
- Multiple protocol support
- Message filtering
- Dead letter queues

### Amazon SQS (Simple Queue Service)
**Message queuing** - Fully managed message queuing service for decoupling applications.

*Key characteristics:*
- Standard and FIFO queues
- Visibility timeout
- Dead letter queues
- Message retention

### Amazon API Gateway
**API management** - Create, publish, maintain, monitor, and secure APIs at any scale.

*Key characteristics:*
- RESTful and WebSocket APIs
- Authentication and authorization
- Request/response transformation
- Caching and throttling

---

[<- Back: Main Note](./README.md) | [Next: Introduction ->](./01-introduction.md)