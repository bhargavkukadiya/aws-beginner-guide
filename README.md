# AWS (Amazon Web Services) — Complete Beginner's Guide

[![Documentation](https://img.shields.io/badge/docs-GitHub%20Pages-orange.svg)](https://bhargavkukadiya.github.io/aws-beginner-guide/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![AWS](https://img.shields.io/badge/AWS-Cloud%20Computing-232F3E?logo=amazon-aws)](https://aws.amazon.com)

> A practical, jargon-free introduction to the world's leading cloud platform.

📖 **Read the interactive, searchable version at [bhargavkukadiya.github.io/aws-beginner-guide](https://bhargavkukadiya.github.io/aws-beginner-guide/)**

---

## Table of Contents

1. [What is AWS?](#1-what-is-aws)
2. [Core Concepts](#2-core-concepts)
3. [Setting Up Your AWS Account](#3-setting-up-your-aws-account)
4. [AWS Global Infrastructure](#4-aws-global-infrastructure)
5. [Compute Services](#5-compute-services)
6. [Storage Services](#6-storage-services)
7. [Networking](#7-networking)
8. [Databases](#8-databases)
9. [AI & Machine Learning Services](#9-ai--machine-learning-services)
10. [Identity & Security (IAM)](#10-identity--security-iam)
11. [Monitoring & Logging](#11-monitoring--logging)
12. [Pricing & Cost Management](#12-pricing--cost-management)
13. [AWS CLI Basics](#13-aws-cli-basics)
14. [Common Beginner Architectures](#14-common-beginner-architectures)
15. [Best Practices](#15-best-practices)
16. [Next Steps & Learning Resources](#16-next-steps--learning-resources)

---

## 1. What is AWS?

**Amazon Web Services (AWS)** is a cloud computing platform offered by Amazon. Instead of buying and maintaining your own physical servers, you rent computing resources — servers, storage, databases, networking, and more — over the internet, paying only for what you use.

### Why Use AWS?

| Traditional IT | AWS Cloud |
|---|---|
| Buy expensive hardware upfront | Pay only for what you use |
| Capacity fixed at purchase | Scale up or down in minutes |
| Weeks to provision servers | Servers ready in seconds |
| You manage physical hardware | AWS manages the data centers |
| One location | 37+ global regions |

### Key Cloud Computing Models

- **IaaS (Infrastructure as a Service)** — Raw compute, storage, networking (e.g., EC2, S3, VPC)
- **PaaS (Platform as a Service)** — Managed platforms to deploy apps without managing OS (e.g., Elastic Beanstalk, App Runner, RDS)
- **SaaS (Software as a Service)** — Fully managed software accessed via browser (e.g., AWS WorkMail)

---

## 2. Core Concepts

### Cloud Deployment Models

| Model | Description | Example |
|---|---|---|
| **Public Cloud** | Resources shared across customers, hosted by AWS | Standard AWS usage |
| **Private Cloud** | Dedicated infrastructure for one organization | AWS Outposts |
| **Hybrid Cloud** | Mix of on-premises + cloud | AWS Direct Connect |

### Key Terminology

- **Instance** — A virtual server running in the cloud
- **Resource** — Any AWS component you create (server, database, bucket, etc.)
- **Service** — A category of cloud functionality (e.g., EC2, S3, RDS)
- **Console** — The web-based dashboard to manage AWS resources
- **CLI** — Command Line Interface for managing AWS from your terminal
- **SDK** — Software Development Kit to use AWS from your code (Python, JavaScript/TypeScript, Go, Java, etc.)
- **ARN** — Amazon Resource Name; a unique identifier for every AWS resource

---

## 3. Setting Up Your AWS Account

### Step 1: Create a Free Account

1. Go to [https://aws.amazon.com](https://aws.amazon.com)
2. Click **Create an AWS Account**
3. Provide your email, password, and account name
4. Enter billing information (credit/debit card required, but the Free Tier covers most beginner usage)
5. Verify your identity via phone/SMS

### Step 2: Secure Your Root Account

Your **root account** has unrestricted access. Secure it immediately:

- Enable **Multi-Factor Authentication (MFA)** on the root account
- Never use the root account for daily tasks
- Create an **IAM user** or **IAM Identity Center user** with appropriate permissions for daily use (see Section 10)

### Step 3: Set a Billing Alarm

Avoid surprise bills:

1. Go to **CloudWatch** → **Alarms** → **Billing** (or **AWS Budgets**)
2. Create an alarm for `$5` or `$10` to get an email notification

### AWS Free Tier (Current Model: July 15, 2025 Onward)

At signup you choose a **Free Plan** or **Paid Plan**:

- **$100 in credits** automatically at signup
- Up to **$100 more** by completing onboarding activities (e.g., launching an EC2 instance, trying Amazon Bedrock, setting an AWS Budget)
- **Credit Validity:** Credits are valid for **12 months** from signup
- **Free Plan Lifespan:** The Free Plan concludes after **6 months** (or upon credit exhaustion). AWS then closes the account and removes resource access, retaining data for a **90-day grace period** to upgrade before deletion
- **Paid Plan:** If upgraded to or started on Paid, standard on-demand billing begins once credits are exhausted
- The old "750 hours/month for 12 months" style allowances (EC2, RDS, etc.) have been retired for new signups — that usage now simply draws down your credit balance

**Always Free (All accounts, never expires):**

| Service | Free Tier Allowance |
|---|---|
| Lambda | 1 million requests/month + 400,000 GB-seconds compute/month (3.2M sec at 128MB RAM) |
| DynamoDB | 25 GB storage + 25 WCU / 25 RCU |
| CloudWatch | 10 metrics, 10 alarms, 5 GB logs |
| SNS & SQS | 1,000,000 messages / requests/month each |

> *Historical Note:* The legacy 12-month tier offered prior to July 15, 2025 has concluded for all accounts; all active accounts operate under the credit model or standard billing + Always Free.

---

## 4. AWS Global Infrastructure

AWS runs on a massive network of data centers worldwide. Understanding the structure helps you design resilient, fast applications.

### Regions

A **Region** is a geographic area (e.g., `us-east-1` = N. Virginia, `ap-south-1` = Mumbai). Each region is completely independent. As of 2026, AWS operates **37+ Regions** worldwide (including dedicated digital sovereignty regions like the **AWS European Sovereign Cloud**).

**How to choose a region:**
- Proximity to your users (lower latency)
- Data residency requirements (legal compliance)
- Service availability (not all services exist in all regions)
- Pricing (varies by region)

### Availability Zones (AZs)

Each Region contains a **minimum of three Availability Zones** (e.g., `us-east-1a`, `us-east-1b`). Each AZ consists of one or more physically separate data centers with independent power, cooling, and networking, situated far enough apart (~60 miles / 100 km) to avoid correlated failures, but close enough for synchronous replication. AWS operates **100+ Availability Zones** globally.

> **Best practice:** Deploy your application across multiple AZs so if one fails, the others keep running.

### Edge Locations

AWS runs a global edge network of **750+ Points of Presence** (Edge Locations and regional caches) across 100+ cities in 50+ countries. These cache content closer to users via **CloudFront** (AWS's CDN), reducing latency for websites, APIs, and media delivery.

### AWS Local Zones

**Local Zones** are extensions of a Region placed in major metro areas, letting you run latency-sensitive workloads (real-time gaming, media production, ML inference) closer to end users.

### Common Region Codes

| Region | Code |
|---|---|
| US East (N. Virginia) | `us-east-1` |
| US West (Oregon) | `us-west-2` |
| Europe (Ireland) | `eu-west-1` |
| Asia Pacific (Mumbai) | `ap-south-1` |
| Asia Pacific (Sydney) | `ap-southeast-2` |

---

## 5. Compute Services

Compute = running code / programs in the cloud.

### EC2 — Elastic Compute Cloud

EC2 provides **virtual machines** (instances) where you can run any OS and software.

#### Instance Types

EC2 instances are named like `t4g.micro`, `m7i.large`, `c8a.xlarge`:

- **First letter** = family (t=burstable, m=balanced, c=compute, r=memory, g=GPU)
- **Number** = generation
- **After the dot** = size (nano, micro, small, medium, large, xlarge, 2xlarge...)

| Family | Best For | Example |
|---|---|---|
| T | Low-cost general use, dev/test | t4g.micro |
| M | Balanced compute + memory | m7i.large / m9g.large |
| C | CPU-intensive workloads | c8a.xlarge |
| R | Memory-intensive workloads | r7g.large |
| G | Graphics-intensive, ML inference | g6.xlarge |
| P / Trn / Inf | AI/ML training and inference (GPU / custom silicon) | p5.48xlarge, trn3, inf2 |

> A lowercase **`g`** (e.g., `m7g`, `t4g`) means it runs on AWS's own **Graviton** ARM-based chips, which offer up to 40% better price-performance than comparable x86 instances. **Graviton5** powers the `M9g` family.

#### Purchasing Options

| Option | Description | Savings |
|---|---|---|
| **On-Demand** | Pay by the hour/second, no commitment | None (baseline) |
| **Reserved** | 1 or 3 year commitment | Up to 72% |
| **Spot** | Bid for unused capacity | Up to 90% |
| **Savings Plans** | Flexible commitment to usage amount | Up to 66% |

#### Launching an EC2 Instance (Console)

1. Go to **EC2** → **Launch Instance**
2. Choose an **AMI** (Amazon Machine Image) — e.g., Amazon Linux 2023, Ubuntu
3. Choose an **instance type** (e.g., `t3.micro` or `t4g.micro`)
4. Configure **storage** (root volume)
5. Configure a **Security Group** — rules for allowed traffic
6. Create or select a **Key Pair** — `.pem` file used for SSH access
7. Click **Launch**

#### Connecting to EC2 (SSH)

```bash
# Make your key private
chmod 400 your-key.pem

# Connect to Linux instance
ssh -i "your-key.pem" ec2-user@<public-ip-or-dns>
```

---

### Lambda — Serverless Functions

AWS Lambda lets you **run code without managing servers**. You pay only when your code runs.

- Natively supports Python, Node.js, Java, .NET, and Ruby (Go and Rust are supported via lightweight custom runtimes like `provided.al2023`)
- Automatically scales — from 1 to millions of executions
- Max execution time: 15 minutes
- Triggered by events (HTTP request, file upload, SQS message, schedule, database change)
- Supports ARM64 (Graviton) architecture for lower execution costs

#### Simple Lambda Function (Python)

```python
def lambda_handler(event, context):
    name = event.get("name", "World")
    return {
        "statusCode": 200,
        "body": f"Hello, {name}!"
    }
```

---

### Elastic Beanstalk & App Runner

- **Elastic Beanstalk:** A **PaaS** service — upload your application code and AWS handles provisioning EC2, load balancers, auto-scaling, and monitoring automatically.
- **App Runner:** A fully managed service for deploying containerized web applications and APIs with automatic HTTPS and zero infrastructure configuration.

---

### ECS & EKS — Container Services

- **ECS (Elastic Container Service)** — Run Docker containers, managed by AWS
- **EKS (Elastic Kubernetes Service)** — Fully managed Kubernetes
- **Fargate** — Serverless containers (no EC2 management needed)

---

## 6. Storage Services

### S3 — Simple Storage Service

S3 stores **files (objects)** in **buckets** (like folders). It's infinitely scalable, highly durable (99.999999999% — 11 nines), has strong read-after-write consistency, and is ideal for hosting static files, backups, and data lakes.

#### Storage Classes

| Class | Use Case | Cost |
|---|---|---|
| S3 Standard | Frequently accessed data | Standard |
| S3 Intelligent-Tiering | Unknown/changing access patterns | Automatic (zero retrieval fee) |
| S3 Standard-IA | Infrequently accessed | Lower storage |
| S3 Express One Zone | Latency-critical AI/analytics | Optimized for sub-10ms IOPS |
| S3 Glacier Instant | Archives, ms retrieval | Much lower |
| S3 Glacier Flexible | Standard archive | Extremely low |
| S3 Glacier Deep Archive | Long-term cold storage | Lowest ($0.00099/GB/mo) |

#### Basic S3 Operations (CLI)

```bash
# Create a bucket
aws s3 mb s3://my-unique-bucket-name

# Upload a file
aws s3 cp myfile.txt s3://my-bucket/

# List objects in a bucket
aws s3 ls s3://my-bucket/

# Download a file
aws s3 cp s3://my-bucket/myfile.txt ./

# Sync a folder
aws s3 sync ./local-folder s3://my-bucket/folder/

# Delete an object
aws s3 rm s3://my-bucket/myfile.txt
```

#### Hosting a Static Website

- **Production Standard (CloudFront + Private S3 with OAC):** Keep S3 *Block Public Access* enabled and Static Website Hosting disabled. Route traffic through CloudFront using **Origin Access Control (OAC)** for HTTPS caching and security.
- **Direct S3 Website Endpoint:** Enable *Static Website Hosting* under Bucket Properties and attach a public read policy to serve files over the S3 HTTP website endpoint.

---

### EBS — Elastic Block Store

**EBS** volumes are virtual hard drives attached to EC2 instances. They persist independently of the instance lifecycle.

- **`gp3` is the recommended default** (provides 3,000 IOPS and 125 MB/s throughput independently of volume size, 20% cheaper than `gp2`).
- Attached to EC2 instances within the same Availability Zone.
- Backed up durably via EBS Snapshots to Amazon S3.

---

### EFS — Elastic File System

**EFS** is a managed, serverless **NFS file system** that can be mounted on multiple EC2 instances, containers, and Lambda functions simultaneously across multiple AZs.

---

### Storage Comparison

| Service | Type | Use Case |
|---|---|---|
| S3 | Object Storage | Files, backups, static assets, data lakes |
| EBS | Block Storage | EC2 root volumes, databases |
| EFS | File Storage | Shared files across multiple instances |
| Glacier | Archival | Long-term compliant backup |

---

## 7. Networking

### VPC — Virtual Private Cloud

A **VPC** is your own isolated virtual network within AWS.

#### Key Components

- **Subnet** — A range of IP addresses within a VPC
  - **Public subnet** — has a direct route to an Internet Gateway
  - **Private subnet** — no direct route to the internet (more secure)
- **Internet Gateway (IGW)** — Connects your VPC to the internet
- **Route Table** — Rules that determine where network traffic goes
- **NAT Gateway** — Lets private subnet resources access the internet (outbound only)
- **Security Group** — Stateful firewall at the instance level (allow rules only)
- **Network ACL (NACL)** — Stateless firewall at the subnet level (allow + deny rules)

#### CIDR Notation & The 5 Reserved IPs

| CIDR | Range | Total IPs | Usable IPs |
|---|---|---|---|
| `10.0.0.0/16` | 10.0.0.0 – 10.0.255.255 | 65,536 | 65,531 |
| `10.0.1.0/24` | 10.0.1.0 – 10.0.1.255 | 256 | 251 |
| `10.0.1.0/28` | 10.0.1.0 – 10.0.1.15 | 16 | 11 |

> **Note:** AWS reserves 5 IP addresses in every subnet (`.0` network, `.1` VPC router, `.2` DNS server, `.3` future use, and the last address in the subnet as broadcast — e.g., `.255` for `/24` or `.15` for `/28`).

---

### Route 53 — DNS Service

Route 53 is AWS's DNS service. Use it to register domains, route traffic to AWS resources, and perform automated health-check failovers.

---

### CloudFront — CDN

**CloudFront** is AWS's Content Delivery Network. It caches your content at 750+ Edge Locations worldwide, reducing latency for global users.

---

### Elastic Load Balancer (ELB)

| Type | Use Case |
|---|---|
| **ALB** (Application LB) | HTTP/HTTPS, path/host-based routing, microservices |
| **NLB** (Network LB) | TCP/UDP, ultra-low latency, static IPs |
| **GLB** (Gateway LB) | Third-party security & network appliances |

---

## 8. Databases

### RDS — Relational Database Service

Managed SQL databases (PostgreSQL, MySQL, MariaDB, Oracle, SQL Server).
- **Multi-AZ** — Automatic failover to standby replica in another AZ
- **Read Replicas** — Scale read traffic across copies
- **Automated Backups** — Point-in-time recovery

---

### Amazon Aurora

AWS's cloud-native relational database compatible with MySQL and PostgreSQL (up to 5x faster than MySQL, 3x faster than PostgreSQL). Scales storage up to 128 TB. **Aurora Serverless v2** auto-scales compute instantly.

---

### DynamoDB — NoSQL Database

A fully managed, **serverless key-value and document database** with single-digit millisecond latency.
- Always free tier: 25 GB storage + 25 Write Capacity Units (WCU) and 25 Read Capacity Units (RCU)

---

### ElastiCache

Managed **in-memory caching** using **Valkey**, **Redis**, or **Memcached**.

---

### Database Comparison

| Service | Type | Best For |
|---|---|---|
| RDS | Relational (SQL) | Traditional apps, complex SQL queries |
| Aurora | Relational (SQL) | High performance, enterprise workloads |
| DynamoDB | NoSQL (key-value) | High-scale, simple access patterns, serverless |
| ElastiCache | In-memory (Valkey/Redis) | Caching, sessions, leaderboards |
| Redshift | Data Warehouse | Analytics, big data OLAP queries |

---

## 9. AI & Machine Learning Services

### Amazon Bedrock

Unified API access to foundation models from Anthropic (Claude), Meta (Llama), Mistral AI, and Amazon (Nova).
- **Bedrock AgentCore** — Autonomous AI agent orchestration (memory, identity, policy controls)
- **Security & Privacy:** Customer data is never used to train base models and is encrypted in transit and at rest. For private VPC networking, configure an **AWS PrivateLink interface VPC endpoint**.

### Amazon SageMaker

Comprehensive platform for custom ML model building, training, and deployment:
- **SageMaker HyperPod** — Cluster infrastructure for large-scale distributed training
- **SageMaker Serverless MLflow** — Experiment tracking without managing servers

### Amazon Nova & Amazon Q

- **Amazon Nova:** AWS's native family of foundation models for text and multimodal reasoning (Nova Micro, Lite, Pro).
- **Amazon Q Developer:** Generative AI coding and cloud troubleshooting assistant.

---

## 10. Identity & Security (IAM)

### Core IAM Components

- **User:** Individual person or legacy identity
- **Group:** Collection of users sharing policies
- **Role:** Assumed temporarily by trusted entities (specific users, roles, or AWS services) authorized by its trust policy
- **Policy:** JSON document defining allowed/denied actions

> **The Principle of Least Privilege:** Always grant the **minimum permissions** needed to do the job.

### Modern Authentication: IAM Identity Center (SSO)

AWS recommends **IAM Identity Center** over static IAM user access keys. Developers log in via browser (`aws sso login`) and receive temporary, auto-expiring session credentials.

---

## 11. Monitoring & Logging

### CloudWatch

Observability service providing **Metrics** (CPU, memory, traffic), **Logs** (application/system log aggregation), **Alarms** (automated threshold alerts), and visual **Dashboards**.

---

### CloudTrail

Governance and compliance auditing:
- **Management Events:** Enabled by default with 90-day free event history.
- **Data Events:** High-volume operations (S3 object access, Lambda invocations) configured via custom Trails.
- **Immutability:** Protect audit logs using **CloudTrail Log File Validation** and S3 Object Lock.

---

### AWS Config & EventBridge

- **AWS Config:** Tracks configuration history and validates resource compliance against security rules.
- **Amazon EventBridge:** Serverless event bus for reacting to AWS state changes and scheduling tasks.

---

## 12. Pricing & Cost Management

- **AWS Cost Explorer:** Analyze historical spending trends and forecasts
- **AWS Budgets:** Custom cost/usage email thresholds
- **Cost Allocation Tags:** Track costs by project/team
- **Billing Alarms:** Proactively set up a `$5`–`$10` CloudWatch alert

---

## 13. AWS CLI Basics

```bash
# Verify installation
aws --version

# Modern SSO Login
aws configure sso
aws sso login

# List S3 buckets
aws s3 ls

# List running EC2 instances
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"

# Invoke Lambda function (AWS CLI v2 string payload)
aws lambda invoke \
  --function-name myFunction \
  --cli-binary-format raw-in-base64-out \
  --payload '{"key": "value"}' \
  output.json

# Identity verification
aws sts get-caller-identity
```

---

## 14. Common Beginner Architectures

### Architecture 1: Secure Static Website
```
User → Route 53 (DNS) → CloudFront (CDN + ACM SSL) → S3 (Private Bucket with OAC)
```

### Architecture 2: Simple Web Application
```
User → Load Balancer (ALB) → EC2 Instance(s) in Private Subnet → RDS (PostgreSQL/MySQL)
```

### Architecture 3: Serverless API
```
Mobile/Web App → API Gateway → Lambda → DynamoDB
```

### Architecture 4: Highly Available Web App
```
Route 53 → ALB → Auto Scaling EC2 (across AZ-1a & AZ-1b) → RDS Multi-AZ (Primary + Standby)
```

---

## 15. Best Practices

- **Security:** Enable MFA, use IAM Roles, apply least privilege, keep databases private, enable CloudTrail.
- **Cost:** Set billing alarms, adopt Graviton (ARM64), delete idle resources (unattached EBS/EIPs), use Savings Plans.
- **Reliability:** Deploy across multiple Availability Zones, use Auto Scaling, enable RDS Multi-AZ.
- **Operations:** Use Infrastructure as Code (**AWS CDK** / Terraform) and tag all resources.

---

## 16. Next Steps & Learning Resources

### AWS Certification Roadmap (2026)

```
Cloud Practitioner  /  AI Practitioner   (Foundational)
        ↓
Solutions Architect Associate  /  Developer Associate  /
CloudOps Engineer Associate (SOA-C03)  /  Data Engineer Associate  /
Machine Learning Engineer Associate
        ↓
Solutions Architect Professional  /  DevOps Engineer Professional  /
Generative AI Developer – Professional
        ↓
Specialty Certifications (Security, Advanced Networking)
```

### Free Learning Resources

| Resource | URL |
|---|---|
| AWS Free Tier | https://aws.amazon.com/free |
| AWS Skill Builder | https://skillbuilder.aws |
| AWS Documentation | https://docs.aws.amazon.com |
| AWS Architecture Center | https://aws.amazon.com/architecture |
| AWS Pricing Calculator | https://calculator.aws |

---

*Last updated: 2026 | AWS services and pricing change frequently — always verify at [https://aws.amazon.com](https://aws.amazon.com)*
