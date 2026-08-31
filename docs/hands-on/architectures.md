# Common Beginner Architectures

These reference architecture patterns represent common real-world building blocks used across modern cloud deployments.

---

## Architecture 1: Secure Static Website

```mermaid
graph LR
    User([User / Browser]) --> Route53[Amazon Route 53<br>DNS Resolution]
    User --> CloudFront[Amazon CloudFront<br>Global Edge CDN]
    ACM[AWS Certificate Manager<br>Free SSL/TLS Certificate] -.->|Attaches to| CloudFront
    CloudFront -->|Origin Access Control OAC| S3[Amazon S3<br>Private HTML/CSS/JS Assets]
```

- **Services Used:** Amazon S3, Amazon CloudFront, Amazon Route 53, AWS Certificate Manager (ACM).
- **Security Pattern:** The S3 bucket has *Block Public Access* fully enabled. Only CloudFront is authorized via **Origin Access Control (OAC)** to fetch objects.
- **Estimated Cost:** Typically **<\$1.00/month** for low-to-medium traffic websites (often covered entirely by the Free Tier).

---

## Architecture 2: Traditional Three-Tier Web Application

```mermaid
graph TD
    User([User]) --> ALB[Application Load Balancer<br>Public Subnet]
    
    subgraph VPC["Amazon VPC"]
        subgraph PublicSubnets["Public Subnets"]
            ALB
            NAT[NAT Gateway]
        end
        subgraph PrivateAppSubnets["Private App Subnets"]
            EC2_A[EC2 Node 1<br>App Server]
            EC2_B[EC2 Node 2<br>App Server]
        end
        subgraph PrivateDataSubnets["Private Database Subnets"]
            RDS[(Amazon RDS PostgreSQL / MySQL)]
        end
    end
    
    ALB --> EC2_A
    ALB --> EC2_B
    EC2_A --> RDS
    EC2_B --> RDS
    EC2_A -.-> NAT
    EC2_B -.-> NAT
```

- **Services Used:** Amazon VPC, Application Load Balancer, Amazon EC2, Amazon RDS, NAT Gateway, Security Groups.
- **Key Design Principle:** The application servers and databases are kept strictly in private subnets with no public IP addresses. Only the ALB is exposed to the internet.

---

## Architecture 3: Fully Serverless API

```mermaid
graph LR
    Client([Web / Mobile App]) --> APIGW[Amazon API Gateway<br>REST / HTTP Endpoint]
    APIGW --> Lambda[AWS Lambda<br>Serverless Business Logic]
    Lambda --> DynamoDB[(Amazon DynamoDB<br>Serverless NoSQL Storage)]
```

- **Services Used:** Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon CloudWatch.
- **Key Benefits:** Zero servers to manage, automated zero-to-peak scaling, pay exclusively per invocation, and minimal idle cost.

---

## Architecture 4: Highly Available, Resilient Web Architecture

```mermaid
graph TD
    User([User]) --> Route53[Amazon Route 53 DNS]
    
    subgraph VPC["Amazon VPC (Spanning 2 Availability Zones)"]
        subgraph PublicSubnets["Public Subnets (Multi-AZ)"]
            ALB[Application Load Balancer<br>Spans Public Subnets in AZ A & B]
        end
        
        subgraph AZ1["Availability Zone A (Private App & Data Subnets)"]
            EC2_1[EC2 Instance]
            RDS_Pri[(RDS Primary Instance)]
        end
        
        subgraph AZ2["Availability Zone B (Private App & Data Subnets)"]
            EC2_2[EC2 Instance]
            RDS_Stby[(RDS Standby Replica)]
        end
        
        ASG[Auto Scaling Group<br>Maintains healthy instances across AZs] -.-> EC2_1
        ASG -.-> EC2_2
        RDS_Pri == Synchronous Replication ==> RDS_Stby
        ALB --> EC2_1
        ALB --> EC2_2
        EC2_1 --> RDS_Pri
        EC2_2 --> RDS_Pri
    end
    
    Route53 --> ALB
```

- **Services Used:** Amazon Route 53, Application Load Balancer (in public subnets), EC2 in Auto Scaling Group (in private app subnets), Amazon RDS Multi-AZ (in private database subnets), Amazon VPC.
- **Failover Mechanism:** 
    - **Compute Layer:** The ALB continuously performs health checks on individual EC2 instances. If an instance or an entire AZ suffers an outage, the ALB automatically stops sending traffic to unhealthy targets and routes all requests exclusively to healthy instances in the surviving AZ. The Auto Scaling Group concurrently provisions replacement instances.
    - **Database Layer:** If the primary RDS database node fails, Amazon RDS Multi-AZ automatically promotes the synchronous standby replica in AZ B to primary within 60–120 seconds, updating DNS records automatically with zero data loss.
