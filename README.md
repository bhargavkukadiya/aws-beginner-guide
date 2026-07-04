# AWS (Amazon Web Services) — Complete Beginner's Guide

> A practical, jargon-free introduction to the world's leading cloud platform.

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
| One location | 33+ global regions |

### Key Cloud Computing Models

- **IaaS (Infrastructure as a Service)** — Raw compute, storage, networking (e.g., EC2, S3)
- **PaaS (Platform as a Service)** — Managed platforms to deploy apps without managing OS (e.g., Elastic Beanstalk, RDS)
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
- **SDK** — Software Development Kit to use AWS from your code (Python, Swift, Java, etc.)
- **ARN** — Amazon Resource Name; a unique identifier for every AWS resource

---

## 3. Setting Up Your AWS Account

### Step 1: Create a Free Account

1. Go to [https://aws.amazon.com](https://aws.amazon.com)
2. Click **Create an AWS Account**
3. Provide your email, password, and account name
4. Enter billing information (credit card required, but the Free Tier covers most beginner usage)
5. Verify your identity via phone

### Step 2: Secure Your Root Account

Your **root account** has unrestricted access. Secure it immediately:

- Enable **Multi-Factor Authentication (MFA)** on the root account
- Never use the root account for daily tasks
- Create an **IAM user** with appropriate permissions for daily use (see Section 9)

### Step 3: Set a Billing Alarm

Avoid surprise bills:

1. Go to **CloudWatch** → **Alarms** → **Billing**
2. Create an alarm for `$5` or `$10` to get an email notification

### AWS Free Tier (Updated Model)

> **Important:** AWS overhauled the Free Tier on July 15, 2025. The version you get depends on when your account was created.

**Accounts created on or after July 15, 2025 (current model):**

At signup you choose a **Free Plan** or **Paid Plan**. Both start with the same credits:

- **$100 in credits** automatically at signup
- Up to **$100 more** by completing onboarding activities (e.g., launching an EC2 instance, trying Amazon Bedrock, setting an AWS Budget)
- Credits are valid for **6 months** (Free Plan) or **12 months from signup** if you upgrade to Paid
- **Free Plan:** if credits run out or 6 months pass, the account closes automatically (no surprise bill), with a 90-day window to upgrade and recover data before deletion
- **Paid Plan:** once credits run out, standard on-demand billing begins immediately
- The old "750 hours/month for 12 months" style allowances (EC2, RDS, etc.) are **gone** for these accounts — that usage now simply draws down your credit balance

**Accounts created before July 15, 2025 (legacy model):**

You keep the traditional 12-month allowances:

| Service | Free Tier Allowance |
|---|---|
| EC2 | 750 hrs/month (t2.micro or t3.micro) |
| S3 | 5 GB storage |
| RDS | 750 hrs/month (db.t2.micro) |
| EBS | 30 GB storage |

**Always Free (both models, never expires):**

| Service | Free Tier Allowance |
|---|---|
| Lambda | 1 million requests/month |
| DynamoDB | 25 GB storage |
| CloudWatch | 10 metrics, 10 alarms |

Over 30 services offer some form of "Always Free" usage. Because the rules changed mid-2025, always confirm your account type on the **Billing → Free Tier** page before trusting an older tutorial's numbers — a lot of guides online still describe the pre-2025 model.

---

## 4. AWS Global Infrastructure

AWS runs on a massive network of data centers worldwide. Understanding the structure helps you design resilient, fast applications.

### Regions

A **Region** is a geographic area (e.g., `us-east-1` = N. Virginia, `ap-south-1` = Mumbai). Each region is completely independent. As of mid-2026, AWS operates **37+ Regions** worldwide, with more announced regularly (recent additions include dedicated regions for digital sovereignty, such as the **AWS European Sovereign Cloud**, launched January 2026, which is operated entirely within the EU by EU-based staff).

**How to choose a region:**
- Proximity to your users (lower latency)
- Data residency requirements (some data must stay in specific countries)
- Service availability (not all services exist in all regions)
- Pricing (varies by region)

### Availability Zones (AZs)

Most Regions contain a **minimum of three Availability Zones** (some have 6 or more, though a few older regions only have two) — e.g., `us-east-1a`, `us-east-1b`. Each AZ is one or more physically separate data centers with independent power, cooling, and networking, situated far enough apart (up to ~60 miles / 100 km) to avoid correlated failures, but close enough for low-latency synchronous replication. AWS operates **100+ Availability Zones** globally today.

> **Best practice:** Deploy your application across multiple AZs so if one fails, the others keep running.

### Edge Locations

AWS runs a global edge network of **750+ Points of Presence** (including dedicated Edge Locations and regional caches) across 100+ cities in 50+ countries. These cache content closer to users via **CloudFront** (AWS's CDN), reducing latency for websites, APIs, and media delivery.

### AWS Local Zones

**Local Zones** are extensions of a Region placed in or near major metro areas, letting you run latency-sensitive workloads (real-time gaming, media production, ML inference) closer to end users without needing a full Region nearby.

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

EC2 is the most fundamental AWS service. It provides **virtual machines** (called instances) where you can run any OS and software.

#### Instance Types

EC2 instances are named like `t3.micro`, `m5.large`, `c6i.xlarge`:

- **First letter** = family (t=general, m=general balanced, c=compute, r=memory, g=GPU)
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

> A lowercase **`g`** in the instance size (e.g., `m7g`, `c7g`, `t4g`) means it runs on AWS's own **Graviton** ARM-based chips, which typically offer better price-performance than comparable x86 instances. AWS's newest generation, **Graviton5**, launched in late 2025 and powers the `M9g` instance family.

#### Purchasing Options

| Option | Description | Savings |
|---|---|---|
| **On-Demand** | Pay by the hour, no commitment | None (baseline) |
| **Reserved** | 1 or 3 year commitment | Up to 72% |
| **Spot** | Bid for unused capacity | Up to 90% |
| **Savings Plans** | Flexible commitment to usage amount | Up to 66% |

#### Launching an EC2 Instance (Console)

1. Go to **EC2** → **Launch Instance**
2. Choose an **AMI** (Amazon Machine Image) — the OS template (e.g., Amazon Linux 2023, Ubuntu)
3. Choose an **instance type** (start with `t2.micro` for free tier)
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

- Natively supports Python, Node.js, Java, Go, Ruby, and .NET (Swift and others are supported via Custom Runtimes)
- Automatically scales — from 1 to millions of executions
- Max execution time: 15 minutes
- Triggered by events (HTTP request, file upload, schedule, database change)

**Example use cases:**
- REST API backend
- Process uploaded images/files
- Run scheduled tasks (cron jobs)
- React to database changes

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

### Elastic Beanstalk

A **PaaS** service — upload your application code and AWS handles provisioning EC2, load balancers, auto-scaling, and monitoring automatically.

Supports: Python, Node.js, PHP, Ruby, Java, .NET, Go, Docker.

---

### ECS & EKS — Container Services

- **ECS (Elastic Container Service)** — Run Docker containers, managed by AWS
- **EKS (Elastic Kubernetes Service)** — Fully managed Kubernetes
- **Fargate** — Serverless containers (no EC2 management needed)

---

## 6. Storage Services

### S3 — Simple Storage Service

S3 stores **files (objects)** in **buckets** (like folders). It's infinitely scalable, highly durable (99.999999999% — 11 nines), and ideal for hosting static files, backups, and data lakes.

#### Key Concepts

- **Bucket** — Container for objects; must have a globally unique name
- **Object** — A file + its metadata
- **Key** — The object's path/filename within the bucket
- **Region** — Buckets exist in a specific region

#### Storage Classes

| Class | Use Case | Cost |
|---|---|---|
| S3 Standard | Frequently accessed data | Higher |
| S3 Intelligent-Tiering | Unknown access patterns | Automatic |
| S3 Standard-IA | Infrequently accessed | Lower |
| S3 Glacier Instant | Archives, fast retrieval | Much lower |
| S3 Glacier Deep Archive | Long-term cold storage | Lowest |

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

#### Hosting a Static Website on S3

1. Create a bucket with the same name as your domain
2. Enable **Static Website Hosting** in bucket properties
3. Upload your HTML/CSS/JS files
4. Set a **Bucket Policy** to allow public read access

---

### EBS — Elastic Block Store

**EBS** volumes are like virtual hard drives attached to EC2 instances. They persist independently of the instance lifecycle.

- SSD-backed (gp3, io2) or HDD-backed (st1, sc1). **Note: `gp3` is the recommended default** for almost all workloads because of its balance of price and performance.
- Only attachable to one EC2 instance at a time (except io2 Multi-Attach)
- Automatically replicated within its AZ

---

### EFS — Elastic File System

**EFS** is a managed **NFS file system** that can be mounted on multiple EC2 instances simultaneously — ideal for shared storage between instances.

---

### Storage Comparison

| Service | Type | Use Case |
|---|---|---|
| S3 | Object Storage | Files, backups, static assets |
| EBS | Block Storage | EC2 root volumes, databases |
| EFS | File Storage | Shared files across instances |
| Glacier | Archival | Long-term backup |

---

## 7. Networking

### VPC — Virtual Private Cloud

A **VPC** is your own isolated virtual network within AWS. Think of it as your private data center in the cloud. All resources you create live inside a VPC.

#### Key Components

- **Subnet** — A range of IP addresses within a VPC
  - **Public subnet** — has a route to the internet
  - **Private subnet** — no direct internet access (more secure)
- **Internet Gateway (IGW)** — Connects your VPC to the internet
- **Route Table** — Rules that determine where network traffic goes
- **NAT Gateway** — Lets private subnet resources access the internet (outbound only)
- **Security Group** — Stateful firewall at the instance level (allow rules only)
- **Network ACL (NACL)** — Stateless firewall at the subnet level (allow + deny rules)

#### Default VPC

Every AWS account has a **default VPC** in each region with public subnets. Good for getting started, but create a custom VPC for production workloads.

#### CIDR Notation

IP address ranges use CIDR notation:

| CIDR | Range | Hosts |
|---|---|---|
| `10.0.0.0/16` | 10.0.0.0 – 10.0.255.255 | 65,536 |
| `10.0.1.0/24` | 10.0.1.0 – 10.0.1.255 | 256 |
| `10.0.1.0/28` | 10.0.1.0 – 10.0.1.15 | 16 |

> **Note:** AWS reserves 5 IP addresses in every subnet (for the network address, VPC router, DNS server, future use, and broadcast), so a `/28` actually gives you 11 usable IPs for instances.

---

### Route 53 — DNS Service

Route 53 is AWS's DNS service. Use it to:

- Register domain names
- Route traffic to EC2, S3, CloudFront, load balancers
- Health check endpoints and do failover routing

---

### CloudFront — CDN

**CloudFront** is AWS's Content Delivery Network. It caches your content at Edge Locations worldwide, reducing latency for users far from your origin server.

Common use: serving static assets (images, JS, CSS) from S3 faster globally.

---

### Elastic Load Balancer (ELB)

Automatically distributes incoming traffic across multiple EC2 instances:

| Type | Use Case |
|---|---|
| **ALB** (Application LB) | HTTP/HTTPS, URL-based routing |
| **NLB** (Network LB) | TCP/UDP, ultra-low latency |
| **GLB** (Gateway LB) | Third-party appliances |

---

## 8. Databases

### RDS — Relational Database Service

Managed **SQL databases**. AWS handles backups, patching, failover, and scaling.

Supported engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Amazon Aurora.

#### Key Features
- **Multi-AZ** — Automatic failover to standby replica
- **Read Replicas** — Scale read traffic across copies
- **Automated Backups** — Point-in-time recovery

---

### Amazon Aurora

AWS's own high-performance relational database. Compatible with MySQL and PostgreSQL, but up to 5x faster than standard MySQL and 3x faster than PostgreSQL. Automatically scales storage up to 128 TB.

---

### DynamoDB — NoSQL Database

A fully managed, **serverless key-value and document database**. Extremely fast at any scale (single-digit millisecond latency).

- No servers to manage
- Scales automatically
- Always free tier: 25 GB storage + 25 Write Capacity Units (WCU) and 25 Read Capacity Units (RCU)

**Best for:** Mobile apps, gaming leaderboards, session management, shopping carts.

---

### ElastiCache

Managed **in-memory caching** using Redis or Memcached. Speeds up applications by caching frequently-read data.

---

### Database Comparison

| Service | Type | Best For |
|---|---|---|
| RDS | Relational (SQL) | Traditional apps, complex queries |
| Aurora | Relational (SQL) | High performance, enterprise |
| DynamoDB | NoSQL (key-value) | High-scale, simple access patterns |
| ElastiCache | In-memory | Caching, sessions, leaderboards |
| Redshift | Data Warehouse | Analytics, big data queries |

---

## 9. AI & Machine Learning Services

AI/ML has become a core pillar of AWS, not a side feature — worth knowing even as a beginner.

### Amazon Bedrock

A fully managed service for building generative AI applications without hosting your own models. It gives API access to foundation models from multiple providers (Anthropic's Claude, Amazon's own Nova models, Meta's Llama, Mistral, and others) through a single interface.

- **Bedrock AgentCore** — infrastructure for building and deploying autonomous AI agents in production (memory, identity, policy controls, evaluation)
- No infrastructure to manage — pay per request/token

### Amazon SageMaker

The full platform for building, training, and deploying custom machine learning models — for teams that want to train their own models rather than call a foundation model API.

- **SageMaker HyperPod** — managed infrastructure for large-scale model training
- **SageMaker Serverless MLflow** — experiment tracking without managing servers

### Amazon Nova

AWS's own family of foundation models (text, image, video, speech), available through Bedrock. Positioned as a lower-cost, AWS-native alternative to third-party models for common tasks.

### Getting Started with AI on AWS

1. Go to **Amazon Bedrock** → request access to a model (e.g., Claude, Nova)
2. Use the **Playground** to test prompts in the console
3. Call the model from code using the Bedrock **Converse API** or SDK
4. For agents that take actions (not just answer questions), look at **Bedrock AgentCore**

> **Beginner tip:** Start with Bedrock, not SageMaker. Bedrock requires no ML expertise — you're calling a pre-trained model via API. SageMaker is for when you need to train or fine-tune your own custom models.

---

## 10. Identity & Security (IAM)

### What is IAM?

**IAM (Identity and Access Management)** controls **who** can do **what** in your AWS account. It is free and foundational to all AWS security.

### Core IAM Components

| Component | Description |
|---|---|
| **User** | An individual person or application |
| **Group** | Collection of users sharing the same permissions |
| **Role** | Permissions assumed by services or users temporarily |
| **Policy** | JSON document defining allowed/denied actions |

### The Principle of Least Privilege

> Always grant the **minimum permissions** needed to do the job — nothing more.

### IAM Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

This policy allows reading and writing objects in `my-bucket` only.

### Setting Up an IAM User (Recommended Workflow)

1. Go to **IAM** → **Users** → **Create User**
2. Enable **Console Access** with a password
3. Attach a permission policy (e.g., `AdministratorAccess` for your personal admin user, or a limited policy for specific tasks)
4. Enable **MFA** for the user
5. Generate **Access Keys** if the user needs programmatic/CLI access

### IAM Roles

Roles are used when AWS services need to interact with each other. For example:

- An EC2 instance needs to read from S3 → attach an **IAM Role** to the EC2 instance
- A Lambda function needs to write to DynamoDB → attach an IAM Role to the Lambda

Never hardcode AWS credentials in your code. Always use IAM Roles for service-to-service access.

---

## 11. Monitoring & Logging

### CloudWatch

**CloudWatch** is AWS's monitoring and observability service.

- **Metrics** — Performance data (CPU usage, network traffic, request count)
- **Logs** — Collect and store log files from EC2, Lambda, and other services
- **Alarms** — Trigger notifications or actions when a metric crosses a threshold
- **Dashboards** — Custom visual displays of metrics
- **Events / EventBridge** — React to changes in your AWS environment

#### Common Metrics to Monitor

| Resource | Key Metrics |
|---|---|
| EC2 | CPUUtilization, NetworkIn/Out, DiskReadOps |
| RDS | DatabaseConnections, FreeStorageSpace, ReadLatency |
| Lambda | Invocations, Errors, Duration, Throttles |
| S3 | NumberOfObjects, BucketSizeBytes |

---

### CloudTrail

**CloudTrail** records every API call made in your AWS account — who did what, when, from where. Essential for security auditing and compliance.

---

### AWS Config

Tracks configuration changes to your AWS resources over time. Useful for compliance and security analysis.

---

## 12. Pricing & Cost Management

### How AWS Pricing Works

AWS pricing is based on:

- **Compute** — Per second or per hour for EC2; per invocation for Lambda
- **Storage** — Per GB per month
- **Data Transfer** — Inbound is free; outbound to the internet costs money
- **Requests** — Per API call for services like S3, DynamoDB

### Cost Management Tools

| Tool | Purpose |
|---|---|
| **AWS Cost Explorer** | Visualize and analyze spending |
| **AWS Budgets** | Set custom cost and usage alerts |
| **Billing Dashboard** | See current month charges |
| **Cost Allocation Tags** | Tag resources to track costs by project/team |
| **Pricing Calculator** | Estimate costs before you build |

### Tips to Avoid Surprise Bills

1. Set a **billing alarm** in CloudWatch (as described in Section 3)
2. Delete resources you no longer need (especially EC2 and RDS)
3. Use the **Free Tier** wisely — monitor usage at **Billing → Free Tier**
4. Stop (not terminate) EC2 instances when not in use
5. Use S3 lifecycle policies to move old data to cheaper storage classes

---

## 13. AWS CLI Basics

The **AWS CLI** lets you manage AWS from your terminal — faster than the console for repetitive tasks.

### Installation

```bash
# macOS (Homebrew)
brew install awscli

# Linux
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip && sudo ./aws/install

# Verify
aws --version
```

### Configuration

```bash
aws configure
# Enter:
# AWS Access Key ID
# AWS Secret Access Key
# Default region (e.g., us-east-1)
# Default output format (json / table / text)
```

### Useful CLI Commands

```bash
# List S3 buckets
aws s3 ls

# List EC2 instances
aws ec2 describe-instances

# Start / Stop an EC2 instance
aws ec2 start-instances --instance-ids i-1234567890abcdef0
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Invoke a Lambda function
aws lambda invoke --function-name myFunction output.json

# Get caller identity (who am I?)
aws sts get-caller-identity
```

### CLI Profiles (Multiple Accounts)

```bash
# Configure a named profile
aws configure --profile myproject

# Use a specific profile
aws s3 ls --profile myproject

# Set a default profile for the session
export AWS_PROFILE=myproject
```

---

## 14. Common Beginner Architectures

### Architecture 1: Static Website

```
User → CloudFront (CDN) → S3 (HTML/CSS/JS files)
                    ↓
             Route 53 (DNS)
```

**Services used:** S3, CloudFront, Route 53

**Cost:** Very low — often under $1/month for low traffic sites.

---

### Architecture 2: Simple Web Application

```
User → Load Balancer (ALB)
              ↓
       EC2 Instance(s)
              ↓
         RDS (MySQL)
```

**Services used:** EC2, ALB, RDS, VPC, Security Groups

---

### Architecture 3: Serverless API

```
Mobile/Web App → API Gateway → Lambda → DynamoDB
```

**Services used:** API Gateway, Lambda, DynamoDB

**Benefits:** No servers to manage, scales automatically, pay per request.

---

### Architecture 4: Highly Available Web App

```
                Route 53
                   ↓
        Application Load Balancer
          ↙                ↘
  EC2 (AZ-1a)          EC2 (AZ-1b)
          ↘                ↙
       RDS Primary (AZ-1a)
              ↓ (replication)
       RDS Standby (AZ-1b)
```

**Services used:** Route 53, ALB, EC2 in Auto Scaling Group, RDS Multi-AZ, VPC with public + private subnets

---

## 15. Best Practices

### Security

- Enable MFA on root account and all IAM users
- Never share or hardcode AWS credentials
- Use IAM Roles for service-to-service permissions
- Apply the principle of least privilege
- Enable CloudTrail in all regions
- Regularly audit IAM users and permissions
- Use private subnets for databases and backend services

### Cost

- Set billing alarms before deploying anything
- Right-size instances — don't over-provision
- Use Auto Scaling to scale down when not needed
- Delete idle resources (forgotten snapshots, old EBS volumes, unused Elastic IPs)
- Use Reserved Instances or Savings Plans for predictable workloads
- Enable S3 lifecycle policies

### Reliability

- Deploy across multiple Availability Zones
- Use Auto Scaling Groups for EC2
- Enable RDS Multi-AZ for production databases
- Create regular snapshots/backups
- Test your disaster recovery plan

### Performance

- Use CloudFront for static content delivery
- Use ElastiCache to reduce database load
- Choose the right instance type for your workload
- Use read replicas for read-heavy database workloads

### Operations

- Tag all resources (Project, Environment, Owner, Cost Center)
- Use CloudFormation or Terraform for Infrastructure as Code
- Monitor with CloudWatch and set alarms proactively
- Use separate AWS accounts for dev, staging, and production

---

## 16. Next Steps & Learning Resources

### AWS Certification Roadmap (Beginner → Advanced)

AWS now offers 12 active certifications across four tiers: Foundational, Associate, Professional, and Specialty.

```
Cloud Practitioner  /  AI Practitioner   (Foundational)
        ↓
Solutions Architect Associate  /  Developer Associate  /
CloudOps Engineer Associate  /  Data Engineer Associate  /
Machine Learning Engineer Associate
        ↓
Solutions Architect Professional  /  DevOps Engineer Professional  /
Generative AI Developer – Professional
        ↓
Specialty Certifications (Security, Advanced Networking, etc.)
```

Start with the **AWS Certified Cloud Practitioner** (technical beginners) or **AWS Certified AI Practitioner** (if your interest is generative AI/agents specifically) — both are entry-level and cover the concepts in this guide. If you already work in IT, you can reasonably skip straight to **Solutions Architect Associate**, which is the single most in-demand AWS certification by job postings.

**2026 pricing:** Cloud Practitioner — $100 · Associate-level — $150 · Professional-level — $300 · Specialty — $300. Certifications are valid for 3 years.

> **Note:** AWS has been retiring some older Specialty exams and shifting focus toward AI/ML and data roles. The **Machine Learning – Specialty** exam had its final sitting on March 31, 2026; new candidates in that space should pursue **Machine Learning Engineer – Associate** or **AI Practitioner** instead. Always check the official AWS Certification page for current exam codes before you start studying, since paths change fairly often.

### Free Learning Resources

| Resource | URL |
|---|---|
| AWS Free Tier | https://aws.amazon.com/free |
| AWS Skill Builder | https://skillbuilder.aws |
| AWS Documentation | https://docs.aws.amazon.com |
| AWS Architecture Center | https://aws.amazon.com/architecture |
| AWS YouTube Channel | https://youtube.com/@amazonwebservices |
| AWS Pricing Calculator | https://calculator.aws |

### Recommended Hands-On Projects

1. **Host a static website** on S3 + CloudFront
2. **Launch an EC2 instance**, install a web server, serve a page
3. **Build a serverless API** with Lambda + API Gateway + DynamoDB
4. **Set up a VPC** with public and private subnets
5. **Store and retrieve files** from S3 using the CLI and SDK
6. **Create a CloudWatch alarm** for EC2 CPU usage
7. **Deploy a containerized app** with ECS Fargate

### Key Services Summary

| Category | Core Services |
|---|---|
| Compute | EC2, Lambda, ECS, Fargate, Elastic Beanstalk |
| Storage | S3, EBS, EFS, Glacier |
| Database | RDS, Aurora, DynamoDB, ElastiCache |
| AI / ML | Bedrock, Bedrock AgentCore, SageMaker, Nova |
| Networking | VPC, Route 53, CloudFront, ELB |
| Security | IAM, Security Groups, KMS, WAF |
| Monitoring | CloudWatch, CloudTrail, AWS Config |
| Developer | CLI, SDK, CloudFormation, CodePipeline |

---

*Last updated: July 2026 | AWS services and pricing change frequently — always verify at [https://aws.amazon.com](https://aws.amazon.com)*
