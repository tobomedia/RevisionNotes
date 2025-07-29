# AWS Cloud Practitioner Certification - Complete Study Guide

## Table of Contents
1. [IAM (Identity and Access Management)](#iam-identity-and-access-management)
2. [EC2 (Elastic Compute Cloud)](#ec2-elastic-compute-cloud)
3. [EC2 Storage Solutions](#ec2-storage-solutions)
4. [Amazon S3](#amazon-s3)
5. [Load Balancing & Auto Scaling](#load-balancing--auto-scaling)
6. [Databases & Analytics](#databases--analytics)
7. [Container Services](#container-services)
8. [Serverless Computing](#serverless-computing)
9. [Deployment & Infrastructure Management](#deployment--infrastructure-management)
10. [Global Content Delivery Network](#global-content-delivery-network)
11. [Amazon Lightsail](#amazon-lightsail)
12. [Quick Reference Tables](#quick-reference-tables)
13. [AWS Acronyms Glossary](#aws-acronyms-glossary)

---

## [IAM](#glossary-iam) (Identity and Access Management)

### Overview
**IAM is a global service** that manages access and permissions across all AWS services and regions. It's free to use and fundamental to AWS security.

### Root Account Security ⚠️
- **NEVER use root account for daily tasks**
- **Essential root account setup:**
  - Enable MFA (Multi-Factor Authentication) immediately
  - Create strong, unique password
  - Delete access keys if they exist
  - Use only for account-level tasks (billing, account closure, support plans)

### IAM Components Comparison

| Component | Purpose | Key Characteristics | Best Practices |
|-----------|---------|-------------------|----------------|
| **Users** | Individual people/applications | Can belong to multiple groups | Create individual users, never share credentials |
| **Groups** | Collections of users by role | Cannot contain other groups | Organize by job function (Developers, Admins) |
| **Policies** | JSON permission documents | Define allowed/denied actions | Use least privilege principle |
| **Roles** | Temporary credentials | Assumed by services/users | Preferred for service-to-service access |

### Access Methods Comparison

| Method | Use Case | Authentication | Key Benefits |
|--------|----------|---------------|--------------|
| **Console** | Web interface | Username/password + MFA | User-friendly, visual management |
| **CLI** | Command line | Access keys | Automation, scripting |
| **PowerShell** | Windows environments | Access keys | Retains session state |
| **SDKs** | Application development | Access keys | Programmatic integration |

### Security Best Practices
- **Enable MFA** on root and privileged accounts
- **Apply least privilege** - grant minimum necessary permissions
- **Use groups** to manage permissions efficiently
- **Rotate access keys** regularly
- **Use roles** for service-to-service access
- **Regular audits** using Credential Report and Access Advisor

---

## [EC2](#glossary-ec2) (Elastic Compute Cloud)

### Overview
**[EC2](#glossary-ec2) = Elastic Compute Cloud** - AWS's foundational service for renting virtual servers with complete control over the computing environment.

### Instance Types Comparison

| Family | Purpose | Use Cases | Key Characteristics |
|--------|---------|-----------|-------------------|
| **T (General)** | Balanced performance | Web servers, small databases | Burstable performance, cost-effective |
| **C (Compute)** | CPU intensive | Scientific computing, gaming | High-performance processors |
| **M (Memory)** | Balanced memory | In-memory databases, caching | Balanced compute/memory ratio |
| **R (RAM)** | Memory intensive | Large databases, analytics | High memory-to-vCPU ratio |
| **I/D/H1 (Storage)** | Storage optimized | Data warehouses, NoSQL | High sequential read/write |
| **X (Extreme)** | Highest memory | SAP HANA, Apache Spark | Highest memory instances |
| **Z (High Freq)** | High single-thread | Gaming, HPC | High single-thread performance |

### EC2 Purchasing Options Comparison

| Option | Commitment | Discount | Flexibility | Best For |
|--------|------------|----------|-------------|----------|
| **On-Demand** | None | 0% | Highest | Testing, irregular workloads |
| **Spot** | None | Up to 90% | Can be terminated | Fault-tolerant, batch jobs |
| **Reserved Standard** | 1-3 years | Up to 75% | Cannot change type | Steady-state applications |
| **Reserved Convertible** | 1-3 years | Up to 66% | Can change type | Steady with flexibility needs |
| **Dedicated Hosts** | Optional | Varies | Full host control | Compliance, licensing |
| **Dedicated Instances** | None | Varies | Dedicated hardware | Compliance requirements |
| **Savings Plans** | 1-3 years | Up to 72% | Cross-service flexibility | Consistent usage patterns |

### Security Groups
- **Virtual firewall** controlling instance traffic
- **Allow rules only** - cannot create deny rules
- **Stateful** - return traffic automatically allowed
- **Default**: All inbound blocked, all outbound allowed

#### Common Ports Reference
| Port | Protocol | Service | Use Case |
|------|----------|---------|----------|
| 22 | SSH | Secure Shell | Linux remote access |
| 21 | FTP | File Transfer | File uploads |
| 22 | SFTP | Secure FTP | Secure file transfers |
| 80 | HTTP | Web traffic | Websites |
| 443 | HTTPS | Secure web | Secure websites |
| 3389 | RDP | Remote Desktop | Windows remote access |
| 3306 | MySQL | Database | MySQL connections |
| 5432 | PostgreSQL | Database | PostgreSQL connections |

---

## EC2 Storage Solutions

### Storage Types Comparison

| Storage Type | Persistence | Performance | Attachment | Use Cases |
|-------------|-------------|-------------|------------|-----------|
| **[EBS](#glossary-ebs)** | Persistent | High (varies by type) | One instance* | Boot volumes, databases |
| **Instance Store** | Temporary | Highest | Cannot detach | Cache, temporary processing |
| **[EFS](#glossary-efs)** | Persistent | Good | Multiple instances | Shared file storage |
| **[FSx](#glossary-fsx)** | Persistent | Very High | Multiple instances | Specialized file systems |

*Except Multi-Attach for io1/io2

### EBS Volume Types Comparison

| Type | Performance | IOPS | Use Cases | Boot Volume |
|------|-------------|------|-----------|-------------|
| **gp3/gp2** | Balanced | 3,000-16,000 | General purpose, boot volumes | ✅ |
| **io1/io2** | High | Up to 64,000 | Mission-critical databases | ✅ |
| **st1** | Throughput optimized | N/A | Big data, data warehouses | ❌ |
| **sc1** | Cost optimized | N/A | Archive, infrequent access | ❌ |

### File System Options Comparison

| Service | Protocol | OS Support | Multi-Instance | Pricing Model |
|---------|----------|------------|---------------|---------------|
| **[EFS](#glossary-efs)** | NFS v4.1 | Linux only | ✅ | Pay for use |
| **FSx Lustre** | POSIX | Linux | ✅ | Provisioned |
| **FSx Windows** | SMB | Windows | ✅ | Provisioned |
| **FSx NetApp** | NFS/SMB/iSCSI | Both | ✅ | Provisioned |
| **FSx OpenZFS** | NFS | Linux | ✅ | Provisioned |

### Storage Decision Framework
- **Persistent + Single Instance** → [EBS](#glossary-ebs)
- **Highest Performance + Temporary** → Instance Store
- **Shared Access + Linux** → [EFS](#glossary-efs)
- **Shared Access + Windows** → [FSx](#glossary-fsx) for Windows
- **HPC/ML Workloads** → [FSx](#glossary-fsx) for Lustre

---

## Amazon [S3](#glossary-s3)

### Overview
AWS's object storage service ([S3](#glossary-s3)) with files called **Objects** stored in **Buckets** that are region-specific.

### S3 Storage Classes Comparison

| Storage Class | Access Frequency | Retrieval Time | Cost | Use Cases |
|---------------|------------------|----------------|------|-----------|
| **Standard** | Frequent | Immediate | Highest | Active data, websites |
| **Standard-IA** | Infrequent | Immediate | Lower | Backups, disaster recovery |
| **One Zone-IA** | Infrequent | Immediate | Lowest IA | Non-critical data |
| **Glacier Instant** | Archive | Immediate | Archive pricing | Archive with instant access |
| **Glacier Flexible** | Archive | Minutes to hours | Very low | Long-term backup |
| **Glacier Deep** | Archive | 12-48 hours | Lowest | Long-term archival |
| **Intelligent Tiering** | Variable | Automatic | Automatic optimization | Unknown patterns |

### S3 Features Summary
- **Bucket naming**: 3-63 characters, lowercase, globally unique
- **File size**: Up to 5TB, multipart upload for >5GB
- **Versioning**: Retains multiple versions of objects
- **Replication**: [CRR](#glossary-crr) (Cross Region) and [SRR](#glossary-srr) (Same Region)
- **Encryption**: Server-side or client-side
- **Static website hosting**: Must be manually enabled

---

## Load Balancing & Auto Scaling

### Load Balancer Types Comparison

| Type | OSI Layer | Protocols | Performance | Latency | Static IP | SSL Termination |
|------|-----------|-----------|-------------|---------|-----------|-----------------|
| **[ALB](#glossary-alb)** | Layer 7 | HTTP/HTTPS | ~100K RPS | ~400ms | ❌ DNS only | ✅ Built-in |
| **[NLB](#glossary-nlb)** | Layer 4 | TCP/UDP/TLS | Millions RPS | <100μs | ✅ Per AZ | ✅ Optional |
| **[GLB](#glossary-glb)** | Layer 3 | Any IP | Ultra-high | Minimal | ✅ Per AZ | ❌ Pass-through |

### ALB Routing Capabilities
- **Path-based**: `/api/*` → API servers, `/images/*` → image servers
- **Host-based**: `api.example.com` → API servers
- **Header-based**: Route based on HTTP headers
- **Query parameter**: Route based on URL parameters

### Auto Scaling Strategies Comparison

| Strategy | Trigger | Response Time | Use Cases | Setup Complexity |
|----------|---------|---------------|-----------|------------------|
| **Manual** | Administrator | Immediate | Testing, known events | Simple |
| **Dynamic** | CloudWatch alarms | Minutes | Variable demand | Medium |
| **Target Tracking** | Metric targets | Automatic | Steady optimization | Simple |
| **Scheduled** | Time-based | Predictive | Known patterns | Medium |
| **Predictive** | ML forecasting | Proactive | Historical patterns | Complex |

### Scaling Concepts
- **Vertical Scaling**: Increase instance size (t2.micro → t2.large)
- **Horizontal Scaling**: Add more instances to distribute load
- **High Availability**: Run across multiple AZs for fault tolerance

---

## Databases & Analytics

### Database Types Comparison

| Type | Structure | Scaling | Query Language | Use Cases |
|------|-----------|---------|----------------|-----------|
| **Relational** | Tables with relationships | Vertical primarily | SQL | OLTP, structured data |
| **NoSQL** | Flexible schema | Horizontal | Various | Flexible data, high scale |
| **Graph** | Nodes and relationships | Horizontal | Graph queries | Social networks, recommendations |
| **Time Series** | Time-ordered data | Horizontal | Time-based queries | IoT, monitoring |

### AWS Database Services Comparison

| Service | Type | Engine Options | Serverless | Multi-AZ | Use Cases |
|---------|------|----------------|------------|----------|-----------|
| **[RDS](#glossary-rds)** | Relational | MySQL, PostgreSQL, Oracle, SQL Server | ❌ | ✅ | Traditional applications |
| **[Aurora](#glossary-aurora)** | Relational | MySQL, PostgreSQL compatible | ✅ Available | ✅ | Cloud-native applications |
| **[DynamoDB](#glossary-dynamodb)** | NoSQL | Proprietary | ✅ | ✅ | High-scale web apps |
| **[DocumentDB](#glossary-documentdb)** | Document | MongoDB compatible | ❌ | ✅ | JSON document storage |
| **[Neptune](#glossary-neptune)** | Graph | Gremlin, SPARQL | ❌ | ✅ | Graph applications |
| **[Timestream](#glossary-timestream)** | Time Series | Proprietary | ✅ | ✅ | IoT, monitoring |

### Analytics Services Comparison

| Service | Purpose | Query Method | Data Source | Serverless |
|---------|---------|--------------|-------------|------------|
| **[Redshift](#glossary-redshift)** | Data warehouse | SQL | Various | ✅ Available |
| **[Athena](#glossary-athena)** | Query S3 data | SQL | S3 objects | ✅ |
| **[EMR](#glossary-emr)** | Big data processing | Spark, Hadoop | Various | ❌ |
| **[QuickSight](#glossary-quicksight)** | Business intelligence | Visual interface | Multiple sources | ✅ |
| **[Glue](#glossary-glue)** | ETL processing | Visual/code | Various | ✅ |

### Database Deployment Options

| Option | Purpose | Performance Impact | Cost | Complexity |
|--------|---------|-------------------|------|------------|
| **Read Replicas** | Read scaling | Improves read performance | Medium | Low |
| **Multi-AZ** | High availability | Minimal impact | Medium | Low |
| **Multi-Region** | Disaster recovery | Network latency | High | Medium |

---

## Container Services

### Overview
**Docker** allows lightweight, transportable application environments by packaging applications and their dependencies into containers.

### Container Registry Options

| Registry | Type | Description | Use Cases |
|----------|------|-------------|-----------|
| **Docker Hub** | Public | Public container registry | Open source images, public projects |
| **[ECR](#glossary-ecr)** | Private | Amazon Elastic Container Registry | Private container images, enterprise security |

### Container Orchestration Comparison

| Service | Infrastructure | Management | Kubernetes | Use Cases |
|---------|---------------|------------|------------|-----------|
| **[ECS](#glossary-ecs)** | [EC2](#glossary-ec2) instances | AWS managed | ❌ | AWS-native container orchestration |
| **[Fargate](#glossary-fargate)** | Serverless | AWS managed | ❌ | Serverless containers, no infrastructure management |
| **[EKS](#glossary-eks)** | [EC2](#glossary-ec2)/[Fargate](#glossary-fargate) | User + AWS managed | ✅ | Kubernetes workloads, cloud-agnostic applications |

### Service Characteristics

#### [ECS](#glossary-ecs) (Elastic Container Service)
- **Infrastructure**: Multiple [EC2](#glossary-ec2) instances run within an ECS cluster
- **Management**: AWS handles container orchestration
- **Integration**: Native AWS service integration
- **Learning curve**: Lower for AWS users

#### [Fargate](#glossary-fargate)
- **Infrastructure**: Serverless - no need to provision infrastructure
- **Management**: AWS handles both infrastructure and orchestration
- **Billing**: Pay per task/container usage
- **Use case**: Simplified container deployment

#### [EKS](#glossary-eks) (Elastic Kubernetes Service)
- **Platform**: Open source Kubernetes system
- **Portability**: Cloud-agnostic (can run on AWS, Azure, GCP, on-premises)
- **Flexibility**: Full Kubernetes feature set
- **Hosting options**: Can run on [Fargate](#glossary-fargate) or [ECS](#glossary-ecs)

### Container Decision Framework
- **AWS-native + Simple** → [ECS](#glossary-ecs)
- **No infrastructure management** → [Fargate](#glossary-fargate)
- **Kubernetes expertise + Portability** → [EKS](#glossary-eks)
- **Private container images** → [ECR](#glossary-ecr)

---

## Serverless Computing

### Overview
**Serverless** means developers don't need to manage servers - just deploy code. Also known as **FaaS (Function as a Service)**.

### Serverless Services Examples

| Service | Type | Description |
|---------|------|-------------|
| **Amazon [S3](#glossary-s3)** | Storage | Object storage without server management |
| **[DynamoDB](#glossary-dynamodb)** | Database | NoSQL database with automatic scaling |
| **[Fargate](#glossary-fargate)** | Containers | Container execution without server provisioning |
| **[Lambda](#glossary-lambda)** | Compute | Function execution without server management |

### [Lambda](#glossary-lambda) Deep Dive

#### Key Characteristics
- **Virtual functions** - no server management required
- **Time limited** - maximum 15 minutes execution time
- **On-demand execution** - runs only when triggered
- **Auto-scaled** - AWS handles scaling automatically
- **Event-driven** - triggered by various AWS services

#### Language Support
- Node.js
- Python
- Java
- C# (.NET)
- Ruby
- Go
- PowerShell

#### Pricing Model
- **Pay per request** and compute time
- **Free tier**: 1,000,000 requests and 400,000 GB-seconds compute per month
- **Cost**: $0.20 per million requests after free tier
- **Very cost-effective** for intermittent workloads

#### Common Use Cases
- **Serverless cron jobs** using CloudWatch Events/EventBridge
- **API backends** with [API Gateway](#glossary-api-gateway)
- **Event processing** (file uploads, database changes)
- **Real-time file processing**

### [API Gateway](#glossary-api-gateway)

#### Overview
Routing service for requests to [Lambda](#glossary-lambda) functions and other AWS services.

#### Key Features
- **Serverless and scalable** - no infrastructure management
- **Protocol support** - REST APIs and WebSocket APIs
- **Security features** - Authentication, authorization, API keys
- **Traffic management** - Throttling, caching, monitoring
- **Integration** - Native [Lambda](#glossary-lambda) integration

### [Batch](#glossary-batch)

#### Overview
Fully managed batch processing service for running jobs at any scale.

#### Key Characteristics
- **Batch processing** - jobs have a defined start and end
- **Scalable** - can efficiently run 100,000s of computing jobs
- **Dynamic provisioning** - automatically launches [EC2](#glossary-ec2) instances as needed
- **Docker-based** - batch jobs defined as Docker images

#### [Batch](#glossary-batch) vs [Lambda](#glossary-lambda) Comparison

| Feature | [Lambda](#glossary-lambda) | [Batch](#glossary-batch) |
|---------|------------|--------|
| **Time limit** | 15 minutes maximum | No time limit |
| **Disk space** | Temporary, limited | [EBS](#glossary-ebs)/instance store available |
| **Runtime** | Limited supported runtimes | Any runtime via Docker |
| **Infrastructure** | Serverless | Relies on [EC2](#glossary-ec2) instances |
| **Use cases** | Event-driven, short tasks | Long-running, compute-intensive jobs |

---

## Deployment & Infrastructure Management

### Overview
AWS provides comprehensive tools for managing infrastructure at scale through **Infrastructure as Code (IaC)**, automated deployment pipelines, and systems management services.

### Infrastructure as Code Comparison

| Service | Language Support | Compilation | Availability | Learning Curve | Use Cases |
|---------|------------------|-------------|--------------|----------------|-----------|
| **[CloudFormation](#glossary-cloudformation)** | JSON/YAML | Direct | US-East-1 initially | Medium | AWS-native infrastructure templates |
| **[CDK](#glossary-cdk)** | JavaScript, Python, Java, C#, Go | Compiles to CloudFormation | Global | Higher | Programmatic infrastructure definition |
| **Application Composer** | Visual | Generates CloudFormation | Global | Low | Visual infrastructure design |

### Deployment Services Comparison

| Service | Target Environment | Deployment Type | Management Level | Use Cases |
|---------|-------------------|-----------------|------------------|-----------|
| **[Elastic Beanstalk](#glossary-beanstalk)** | AWS only | Application-focused PaaS | High (AWS managed) | Web applications, API backends |
| **[CodeDeploy](#glossary-codedeploy)** | AWS + On-premises | Code deployment | Medium | Application updates, blue/green deployments |
| **[CodePipeline](#glossary-codepipeline)** | Various targets | CI/CD orchestration | Medium | Complete deployment pipelines |

### [CloudFormation](#glossary-cloudformation)
- **Infrastructure as Code** - Define AWS resources using JSON/YAML templates
- **Repeatable deployments** - Create identical environments multiple times
- **Template sharing** - Reusable infrastructure patterns
- **Initial availability** - Templates deploy to US-East-1 first, then other regions
- **Change management** - Stack updates with rollback capabilities

### Application Composer
- **Visual builder** - Drag-and-drop interface for infrastructure design
- **CloudFormation generation** - Automatically creates CloudFormation templates
- **Real-time preview** - Visual representation of infrastructure relationships
- **Beginner-friendly** - No coding required for basic infrastructure

### [CDK](#glossary-cdk) (Cloud Development Kit)
- **Familiar languages** - Write infrastructure in JavaScript, Python, Java, C#, Go
- **Compilation process** - Code compiles into CloudFormation templates (JSON/YAML)
- **Developer experience** - IDE support, testing, and version control
- **Advanced constructs** - Higher-level abstractions for common patterns

### [Elastic Beanstalk](#glossary-beanstalk)
- **Platform as a Service (PaaS)** - Developer-centric application deployment
- **Architecture models**:
  - **Single instance** - Development and testing environments
  - **Load Balancer + Auto Scaling Group** - Production and pre-production web applications
  - **Auto Scaling Group only** - Non-web applications (workers, batch processing)
- **Health monitoring** - Built-in health agent reports to CloudWatch
- **Application health checks** - Monitors application health and deployment events
- **Easy deployment** - Upload code, Beanstalk handles infrastructure

### Developer Tools Suite

#### [CodeCommit](#glossary-codecommit)
- **Git repository service** - AWS-managed version control (discontinued for new users)
- **GitHub alternative** - Similar functionality to GitHub/GitLab
- **Integration** - Native AWS service integration
- **Note** - May still appear in course materials but discontinued for new customers

#### [CodeDeploy](#glossary-codedeploy)
- **Deployment automation** - Automated application deployments
- **Multi-environment** - Supports AWS cloud and on-premises servers
- **Deployment strategies** - Rolling, blue/green, canary deployments  
- **Application types** - Supports EC2, Lambda, ECS applications

#### [CodePipeline](#glossary-codepipeline)
- **CI/CD orchestration** - End-to-end pipeline automation
- **Source integration** - Works with CodeCommit, GitHub, S3
- **Build integration** - Integrates with CodeBuild, Jenkins, third-party tools
- **Deployment automation** - Coordinates with CodeDeploy and other deployment tools

#### [CodeArtifact](#glossary-codeartifact)
- **Package repository** - Managed artifact repository service
- **Dependency management** - Stores and retrieves software packages
- **Proxy functionality** - Proxies public repositories (npm, PyPI, Maven)
- **Security** - Package scanning and access control

### Systems Management

#### [Systems Manager (SSM)](#glossary-ssm)
- **Hybrid management** - Manages both EC2 and on-premises infrastructure
- **Core capabilities**:
  - **Patch management** - Automated OS and software patching
  - **Run commands** - Execute commands across infrastructure at scale
  - **Session management** - Secure shell access without SSH
- **Agent requirement** - Must install SSM agent on managed instances

#### [SSM Session Manager](#glossary-ssm-session-manager)
- **Secure shell access** - Browser-based terminal sessions
- **No SSH required** - Eliminates need for SSH keys and port 22 access
- **No bastion hosts** - Direct secure access to private instances
- **Audit trail** - Session logs can be sent to S3 for compliance
- **Network isolation** - Works with instances in private subnets

#### [Fleet Manager](#glossary-fleet-manager)
- **Centralized view** - Web portal for all instances with SSM agent
- **Instance management** - Monitor, patch, and manage instances remotely
- **Bulk operations** - Perform actions across multiple instances
- **Health monitoring** - Real-time status of managed instances

#### [Parameter Store](#glossary-parameter-store)
- **Secure storage** - Centralized storage for configuration data and secrets
- **Data types** - API keys, database passwords, configuration settings
- **Version control** - Track parameter changes over time
- **Encryption support** - Automatic encryption for sensitive data
- **Integration** - Native integration with other AWS services
- **Hierarchical organization** - Organize parameters in tree structure

### Deployment Architecture Patterns

#### Beanstalk Architecture Decision Matrix
| Architecture | Use Case | Scalability | Cost | Complexity |
|-------------|----------|-------------|------|------------|
| **Single Instance** | Development, testing | Low | Lowest | Minimal |
| **LB + ASG** | Production web apps | High | Medium | Low |
| **ASG Only** | Background processing, workers | Medium | Low | Low |

#### Infrastructure Management Best Practices
- **Use IaC** - CloudFormation or CDK for reproducible infrastructure
- **Version control** - Track all infrastructure changes
- **Environment parity** - Consistent environments across dev/staging/prod
- **Automated deployment** - Use CodePipeline for CI/CD
- **Centralized configuration** - Parameter Store for application settings
- **Patch management** - Systems Manager for OS updates
- **Monitoring** - CloudWatch integration for operational visibility

### Decision Framework
- **Visual infrastructure design** → Application Composer
- **Template-based infrastructure** → [CloudFormation](#glossary-cloudformation)
- **Code-based infrastructure** → [CDK](#glossary-cdk)
- **Simple application deployment** → [Elastic Beanstalk](#glossary-beanstalk)
- **Complex deployment pipelines** → [CodePipeline](#glossary-codepipeline) + [CodeDeploy](#glossary-codedeploy)
- **Instance management at scale** → [Systems Manager](#glossary-ssm)
- **Secure remote access** → [SSM Session Manager](#glossary-ssm-session-manager)
- **Configuration management** → [Parameter Store](#glossary-parameter-store)

---

## Global Content Delivery Network

### Overview
AWS provides a global network infrastructure designed to bring content, compute, and applications closer to end users worldwide, reducing latency and improving performance through strategic edge locations and hybrid solutions.

### Global Infrastructure Services Comparison

| Service | Primary Purpose | Caching | Network Path | Deployment | Use Cases |
|---------|-----------------|---------|--------------|------------|-----------|
| **[S3 Transfer Acceleration](#glossary-s3-transfer-acceleration)** | Faster S3 uploads | ❌ | AWS backbone | Automatic | Large file uploads to S3 |
| **[Global Accelerator](#glossary-global-accelerator)** | Network optimization | ❌ | AWS backbone | Manual setup | Application performance |
| **[CloudFront](#glossary-cloudfront)** | Content delivery | ✅ | Edge locations | Manual setup | Static/dynamic content delivery |

### Edge Computing Solutions Comparison

| Service | Location | Integration | Latency | Management | Use Cases |
|---------|----------|-------------|---------|------------|-----------|
| **[AWS Outposts](#glossary-outposts)** | On-premises | Full AWS services | Ultra-low | AWS managed | Hybrid cloud, data residency |
| **[Wavelength](#glossary-wavelength)** | 5G provider edge | Limited AWS services | Ultra-low | AWS managed | 5G mobile applications |
| **[Local Zones](#glossary-local-zones)** | Metropolitan areas | VPC extension | Low | AWS managed | Latency-sensitive apps |

### [S3 Transfer Acceleration](#glossary-s3-transfer-acceleration)
- **Purpose** - Significantly speeds up uploads to S3 buckets
- **Mechanism** - Uses CloudFront edge locations to route uploads via AWS backbone
- **Benefits** - Faster data transfer, especially for large files and distant locations
- **Use cases** - Media uploads, backup files, data migration to S3
- **Pricing** - Additional cost only when acceleration provides benefit

### [Global Accelerator](#glossary-global-accelerator)
- **Network optimization** - Routes traffic through AWS's global network infrastructure
- **No caching** - Does not cache content; focuses on optimal routing
- **Anycast IP** - Provides static IP addresses that route to optimal AWS edge location
- **Performance** - Improves performance by up to 60% for TCP/UDP traffic
- **Use cases** - Gaming applications, IoT, VoIP, video streaming
- **Health checks** - Automatic failover to healthy endpoints

### Edge Computing Solutions

#### [AWS Outposts](#glossary-outposts)
- **Hybrid cloud** - Brings AWS infrastructure to on-premises data centers
- **Physical hardware** - Specific server racks deployed at customer premises
- **Full integration** - Extends AWS cloud to on-premises environment
- **Benefits**:
  - **Ultra-low latency** - Processing at the edge
  - **Local data processing** - Compute without cloud round-trips
  - **Data residency** - Keep sensitive data on-premises
  - **Easier migration** - Gradual transition to cloud
- **Management** - Fully managed by AWS
- **Services** - Many AWS services work natively on Outposts

#### [AWS Wavelength](#glossary-wavelength)
- **5G integration** - Wavelength zones embedded within telecommunications providers
- **Edge compute** - Brings AWS services to the edge of 5G networks
- **EC2 at the edge** - Run EC2 instances directly on 5G infrastructure
- **Performance benefits**:
  - **Ultra-low latency** - Single-digit millisecond latency
  - **High bandwidth** - 5G network capabilities
- **Use cases** - Autonomous vehicles, AR/VR, real-time gaming, industrial automation
- **Limited services** - Subset of AWS services available

#### [Local Zones](#glossary-local-zones)
- **Metropolitan extension** - Places compute and storage closer to end users
- **VPC extension** - Extends your Amazon VPC to Local Zone locations
- **Regional connection** - Connected to parent AWS Region via AWS's redundant network
- **Benefits**:
  - **Low latency** - Single-digit millisecond latency for nearby users
  - **Compliance** - Data remains in specific geographic area
  - **Seamless integration** - Works with existing VPC infrastructure
- **Use cases** - Content creation, real-time gaming, machine learning inference

### Global Application Architecture Patterns

#### Multi-Region Active/Passive
- **Active region** - Handles both read and write operations
- **Passive region** - Read-only with data replication from active region
- **Failover** - Manual or automatic failover to passive region during outages
- **Cost optimization** - Lower costs as passive region uses minimal resources
- **Use cases** - Disaster recovery, compliance requirements

#### Multi-Region Active/Active
- **Both regions active** - Both regions handle read and write operations
- **Bi-directional replication** - Data synchronization across both regions
- **Geographic distribution** - Users routed to closest region for optimal performance
- **Higher complexity** - Requires careful data consistency management
- **Higher costs** - Both regions fully operational
- **Use cases** - Global applications, maximum availability requirements

### Performance Optimization Strategies

#### Content Delivery Decision Matrix
| Content Type | Recommendation | Reason |
|-------------|----------------|---------|
| **Static content** | CloudFront | Caching reduces origin load |
| **Dynamic content** | CloudFront + Global Accelerator | Caching + network optimization |
| **Large file uploads** | S3 Transfer Acceleration | Optimized upload path |
| **Real-time applications** | Global Accelerator | Network path optimization |
| **API endpoints** | CloudFront + Global Accelerator | Caching + routing optimization |

#### Edge Computing Decision Matrix
| Requirement | Solution | Key Benefit |
|-------------|----------|-------------|
| **Full AWS services on-premises** | AWS Outposts | Complete cloud experience locally |
| **5G mobile applications** | Wavelength | Ultra-low latency for mobile users |
| **City-level latency reduction** | Local Zones | Metropolitan area optimization |
| **Hybrid cloud strategy** | Outposts | Gradual cloud migration |
| **Data sovereignty** | Outposts or Local Zones | Data remains in specific location |

### Global Infrastructure Best Practices
- **Multi-region strategy** - Deploy across multiple regions for disaster recovery
- **Edge optimization** - Use appropriate edge service for your use case
- **Network monitoring** - Monitor performance across all regions and edge locations
- **Cost optimization** - Balance performance needs with cost considerations
- **Compliance planning** - Consider data residency requirements in architecture decisions
- **Failover testing** - Regularly test failover mechanisms in multi-region setups

### Decision Framework
- **Faster S3 uploads** → [S3 Transfer Acceleration](#glossary-s3-transfer-acceleration)
- **Application performance optimization** → [Global Accelerator](#glossary-global-accelerator)
- **Content caching and delivery** → [CloudFront](#glossary-cloudfront)
- **On-premises AWS services** → [AWS Outposts](#glossary-outposts)
- **5G edge computing** → [Wavelength](#glossary-wavelength)
- **Metropolitan latency reduction** → [Local Zones](#glossary-local-zones)
- **Global disaster recovery** → Multi-region Active/Passive
- **Global high availability** → Multi-region Active/Active

---

## Amazon Lightsail

### Overview
Simplified cloud platform offering virtual servers, storage, databases, and networking with low and predictable pricing.

### Key Features
- **All-in-one solution** - Virtual servers, storage, databases, networking
- **Predictable pricing** - Fixed monthly costs
- **Simplified management** - Easy-to-use interface
- **Target audience** - Users with little cloud experience
- **Use cases** - Simple websites, development environments, small applications

### When to Choose Lightsail
- **Getting started** with cloud computing
- **Predictable costs** are important
- **Simple applications** that don't need complex AWS services
- **Small-scale projects** or personal websites

---

## Quick Reference Tables

### When to Use Which Service

#### Compute
- **Predictable workloads** → Reserved Instances
- **Variable/testing workloads** → On-Demand
- **Fault-tolerant batch jobs** → Spot Instances
- **Compliance requirements** → Dedicated Hosts/Instances

#### Storage
- **Persistent block storage** → [EBS](#glossary-ebs)
- **Highest performance temporary** → Instance Store
- **Shared file access (Linux)** → [EFS](#glossary-efs)
- **Shared file access (Windows)** → [FSx](#glossary-fsx) for Windows
- **HPC/ML workloads** → [FSx](#glossary-fsx) for Lustre

#### Load Balancing
- **HTTP/HTTPS with advanced routing** → [ALB](#glossary-alb)
- **High performance TCP/UDP** → [NLB](#glossary-nlb)
- **Network security appliances** → [GLB](#glossary-glb)

#### Databases
- **Traditional relational** → [RDS](#glossary-rds)
- **Cloud-native relational** → [Aurora](#glossary-aurora)
- **High-scale NoSQL** → [DynamoDB](#glossary-dynamodb)
- **Data warehousing** → [Redshift](#glossary-redshift)
- **Query data in S3** → [Athena](#glossary-athena)

#### Deployment & Infrastructure Management
- **Visual infrastructure design** → Application Composer
- **Template-based infrastructure** → [CloudFormation](#glossary-cloudformation)
- **Code-based infrastructure** → [CDK](#glossary-cdk)
- **Simple application deployment** → [Elastic Beanstalk](#glossary-beanstalk)
- **CI/CD pipelines** → [CodePipeline](#glossary-codepipeline)
- **Automated deployments** → [CodeDeploy](#glossary-codedeploy)
- **Package management** → [CodeArtifact](#glossary-codeartifact)
- **Infrastructure management** → [Systems Manager](#glossary-ssm)
- **Secure remote access** → [SSM Session Manager](#glossary-ssm-session-manager)
- **Configuration management** → [Parameter Store](#glossary-parameter-store)

#### Global Content Delivery & Edge Computing
- **Faster S3 uploads** → [S3 Transfer Acceleration](#glossary-s3-transfer-acceleration)
- **Application performance optimization** → [Global Accelerator](#glossary-global-accelerator)
- **Content caching and delivery** → [CloudFront](#glossary-cloudfront)
- **On-premises AWS services** → [AWS Outposts](#glossary-outposts)
- **5G edge computing** → [Wavelength](#glossary-wavelength)
- **Metropolitan latency reduction** → [Local Zones](#glossary-local-zones)

### Cost Optimization Strategies

| Service | Strategy | Savings | Trade-offs |
|---------|----------|---------|------------|
| **EC2** | Reserved Instances | Up to 75% | 1-3 year commitment |
| **EC2** | Spot Instances | Up to 90% | Can be terminated |
| **[S3](#glossary-s3)** | Intelligent Tiering | Automatic | Small monitoring fee |
| **[S3](#glossary-s3)** | Glacier storage | Up to 80% | Retrieval time/fees |
| **[EBS](#glossary-ebs)** | Snapshot Archive | 75% | 24-72 hour restoration |
| **[EFS](#glossary-efs)** | Infrequent Access | Up to 92% | Retrieval fees |

### Security Best Practices Summary

| Service | Key Security Practices |
|---------|----------------------|
| **[IAM](#glossary-iam)** | MFA, least privilege, regular audits |
| **[EC2](#glossary-ec2)** | Security groups, key pairs, regular patching |
| **[S3](#glossary-s3)** | Bucket policies, encryption, access logging |
| **[RDS](#glossary-rds)** | Security groups, encryption, automated backups |
| **[VPC](#glossary-vpc)** | NACLs, security groups, private subnets |
| **[Systems Manager](#glossary-ssm)** | Session Manager, patch management, secure parameter storage |

### Exam Tips
- **Focus on use cases** - when to use which service
- **Understand cost optimization** - Reserved Instances, Spot, storage classes
- **Know security fundamentals** - [IAM](#glossary-iam), MFA, least privilege
- **Memorize common ports** - 22 (SSH), 80 (HTTP), 443 (HTTPS), 3389 (RDP)
- **Understand high availability** - Multi-AZ deployments
- **Know scaling concepts** - vertical vs horizontal scaling

---

## AWS Acronyms Glossary

### <a id="glossary-alb"></a>ALB - Application Load Balancer
Layer 7 load balancer that operates at the HTTP/HTTPS level, providing advanced routing capabilities based on content, host headers, and URL paths.

### <a id="glossary-api-gateway"></a>API Gateway
Fully managed service that makes it easy to create, publish, maintain, monitor, and secure APIs at any scale. Routes requests to Lambda functions and other AWS services.

### <a id="glossary-athena"></a>Athena
Serverless interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL queries.

### <a id="glossary-batch"></a>Batch
Fully managed batch processing service that efficiently runs hundreds of thousands of computing jobs by dynamically provisioning compute resources based on job requirements.

### <a id="glossary-beanstalk"></a>Elastic Beanstalk
Platform as a Service (PaaS) that provides a developer-centric view for deploying applications on AWS. Supports multiple architecture models including single instance, load balancer with auto scaling, and auto scaling group only configurations.

### <a id="glossary-cdk"></a>CDK - Cloud Development Kit
Infrastructure as code framework that allows you to define cloud resources using familiar programming languages like JavaScript, Python, Java, C#, and Go. Code compiles into CloudFormation templates.

### <a id="glossary-cloudformation"></a>CloudFormation
AWS Infrastructure as Code service that allows you to define AWS resources using JSON/YAML templates. Enables repeatable infrastructure deployments and template sharing across environments.

### <a id="glossary-codeartifact"></a>CodeArtifact
Managed artifact repository service that securely stores and manages software packages. Acts as a proxy for public repositories like npm, PyPI, and Maven while providing package scanning and access control.

### <a id="glossary-codecommit"></a>CodeCommit
AWS-managed Git repository service (discontinued for new users) that provided version control similar to GitHub. May still appear in course materials but is no longer available for new customers.

### <a id="glossary-codedeploy"></a>CodeDeploy
Deployment automation service that supports both AWS cloud and on-premises servers. Provides various deployment strategies including rolling, blue/green, and canary deployments for EC2, Lambda, and ECS applications.

### <a id="glossary-codepipeline"></a>CodePipeline
CI/CD orchestration service that automates the build, test, and deployment phases of your application release process. Integrates with various source control, build, and deployment tools.

### <a id="glossary-aurora"></a>Aurora
MySQL and PostgreSQL-compatible relational database built for the cloud that combines performance and availability of commercial databases with simplicity and cost-effectiveness of open source databases.

### <a id="glossary-cloudfront"></a>CloudFront
AWS's global Content Delivery Network (CDN) service that delivers content to users with low latency by caching data at edge locations worldwide. Supports both static and dynamic content delivery.

### <a id="glossary-global-accelerator"></a>Global Accelerator
Network service that improves the performance of applications by routing traffic through AWS's global network infrastructure. Uses anycast IP addresses and provides up to 60% performance improvement without caching.

### <a id="glossary-crr"></a>CRR - Cross Region Replication
S3 feature that automatically replicates objects across different AWS regions for compliance, lower latency, and disaster recovery.

### <a id="glossary-documentdb"></a>DocumentDB
Fully managed document database service that supports MongoDB workloads, designed for JSON data storage and querying.

### <a id="glossary-dynamodb"></a>DynamoDB
Fully managed NoSQL database service that provides fast and predictable performance with seamless scalability for high-scale applications.

### <a id="glossary-ebs"></a>EBS - Elastic Block Store
High-performance block storage service designed for use with Amazon EC2 for both throughput and transaction intensive workloads.

### <a id="glossary-ecr"></a>ECR - Elastic Container Registry
Fully managed Docker container registry that makes it easy to store, manage, and deploy Docker container images securely.

### <a id="glossary-ecs"></a>ECS - Elastic Container Service
Highly scalable container orchestration service that supports Docker containers and allows you to run applications on a managed cluster of Amazon EC2 instances.

### <a id="glossary-ec2"></a>EC2 - Elastic Compute Cloud
Web service that provides resizable compute capacity in the cloud, allowing you to run virtual servers on demand.

### <a id="glossary-efs"></a>EFS - Elastic File System
Fully managed NFS file system for use with AWS Cloud services and on-premises resources, providing shared storage across multiple instances.

### <a id="glossary-eks"></a>EKS - Elastic Kubernetes Service
Fully managed Kubernetes service that runs Kubernetes control plane across multiple AWS Availability Zones, allowing you to run Kubernetes applications without managing the control plane infrastructure.

### <a id="glossary-emr"></a>EMR - Elastic MapReduce
Cloud big data platform for processing vast amounts of data using open source tools such as Apache Spark, Hadoop, and Presto.

### <a id="glossary-fargate"></a>Fargate
Serverless compute engine for containers that works with both Amazon ECS and EKS, allowing you to run containers without managing servers or clusters.

### <a id="glossary-fleet-manager"></a>Fleet Manager
Web portal that provides a centralized view of all instances with the SSM agent running. Enables bulk operations, monitoring, patching, and remote management of EC2 and on-premises instances.

### <a id="glossary-fsx"></a>FSx - Amazon FSx
Fully managed file systems optimized for compute-intensive workloads, offering high-performance file systems like Lustre and Windows File Server.

### <a id="glossary-glb"></a>GLB - Gateway Load Balancer
Layer 3 load balancer that makes it easy to deploy, scale, and manage virtual appliances like firewalls and intrusion detection systems.

### <a id="glossary-glue"></a>Glue
Fully managed extract, transform, and load (ETL) service that makes it easy to prepare and load data for analytics.

#### Glue Data Catalog
Central metadata repository that stores structural and operational metadata for data assets across AWS. Acts as a persistent metadata store that can be used by AWS analytics services like Athena, EMR, and Redshift Spectrum to discover and query data without needing to know the underlying data format or location.

### <a id="glossary-iam"></a>IAM - Identity and Access Management
Web service that helps you securely control access to AWS resources by managing users, groups, roles, and their permissions.

### <a id="glossary-lambda"></a>Lambda
Serverless compute service that lets you run code without provisioning or managing servers. Functions are event-driven and automatically scale based on demand.

### <a id="glossary-lightsail"></a>Lightsail
Simplified cloud platform that offers virtual servers, storage, databases, and networking with predictable pricing, designed for users with limited cloud experience.

### <a id="glossary-local-zones"></a>Local Zones
AWS infrastructure deployments that place compute, storage, and other AWS services closer to end users in metropolitan areas. Extends VPC to provide single-digit millisecond latency for latency-sensitive applications.

### <a id="glossary-neptune"></a>Neptune
Fully managed graph database service that makes it easy to build and run applications with highly connected datasets.

### <a id="glossary-nlb"></a>NLB - Network Load Balancer
Layer 4 load balancer that handles millions of requests per second with ultra-low latencies while maintaining high throughput.

### <a id="glossary-parameter-store"></a>Parameter Store
Secure, hierarchical storage for configuration data and secrets management. Provides version tracking, encryption support, and integration with other AWS services for storing API keys, database passwords, and configuration settings.

### <a id="glossary-quicksight"></a>QuickSight
Scalable, serverless, embeddable business intelligence service that lets you create and publish interactive dashboards.

### <a id="glossary-rds"></a>RDS - Relational Database Service
Web service that makes it easier to set up, operate, and scale relational databases in the cloud, supporting multiple database engines.

### <a id="glossary-redshift"></a>Redshift
Fully managed, petabyte-scale data warehouse service in the cloud that makes it simple and cost-effective to analyze data.

### <a id="glossary-outposts"></a>AWS Outposts
Hybrid cloud solution that brings native AWS services, infrastructure, and operating models to virtually any on-premises data center. Provides fully managed server racks for ultra-low latency and local data processing.

### <a id="glossary-s3"></a>S3 - Simple Storage Service
Object storage service that offers industry-leading scalability, data availability, security, and performance for storing and retrieving any amount of data.

### <a id="glossary-s3-transfer-acceleration"></a>S3 Transfer Acceleration
Feature that speeds up uploads to S3 buckets by routing traffic through CloudFront edge locations and AWS's backbone network. Provides faster data transfer, especially for large files and distant locations.

### <a id="glossary-srr"></a>SRR - Same Region Replication
S3 feature that automatically replicates objects within the same AWS region for compliance, data protection, and workflow optimization.

### <a id="glossary-ssm"></a>SSM - Systems Manager
Comprehensive service for managing EC2 and on-premises infrastructure at scale. Provides patch management, command execution, session management, and configuration management capabilities.

### <a id="glossary-ssm-session-manager"></a>SSM Session Manager
Component of Systems Manager that provides secure shell access to EC2 instances without requiring SSH keys, bastion hosts, or port 22 access. Sessions can be logged to S3 for audit compliance.

### <a id="glossary-timestream"></a>Timestream
Fully managed time series database service for IoT and operational applications that makes it easy to store and analyze trillions of events per day.

### <a id="glossary-vpc"></a>VPC - Virtual Private Cloud
Logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define.

### <a id="glossary-wavelength"></a>Wavelength
AWS infrastructure deployments embedded within telecommunications providers' 5G networks. Enables ultra-low latency applications by running EC2 instances at the edge of 5G networks for mobile and connected device applications.