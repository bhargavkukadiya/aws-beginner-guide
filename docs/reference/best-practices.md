# AWS Best Practices (Well-Architected)

Following proven cloud design patterns ensures that your workloads are secure, performant, resilient, and cost-efficient.

---

## 1. Security Pillar

- **Enforce MFA Everywhere:** Protect the root account and all individual user identities with Multi-Factor Authentication.
- **Never Hardcode Credentials:** Use **IAM Roles** for EC2, ECS, and Lambda instead of static API access keys.
- **Apply Least Privilege:** Grant only the specific permissions needed for each job.
- **Keep Databases Private:** Never assign public IP addresses to databases; place them in isolated private subnets.
- **Encrypt Data at Rest & in Transit:** Enable default encryption on all S3 buckets, EBS volumes, and RDS databases using **AWS KMS**, and enforce HTTPS via TLS 1.3.
- **Enable CloudTrail Globally:** Maintain an audit trail of all API activity across all AWS Regions.

---

## 2. Cost Optimization Pillar

- **Set Billing Alarms on Day One:** Configure CloudWatch billing alerts and AWS Budgets to prevent unexpected charges.
- **Adopt Graviton (ARM64) Processors:** Switch EC2, RDS, and Lambda workloads to Graviton for an instant ~20–40% price-performance advantage.
- **Clean Up Idle Resources:** Regularly delete unattached EBS volumes, disassociated Elastic IPs, and obsolete DB snapshots.
- **Implement S3 Lifecycle Rules:** Automatically transition older files to `S3 Standard-IA`, `Glacier Flexible Retrieval`, or `Glacier Deep Archive`.
- **Commit with Savings Plans:** Leverage 1-year or 3-year Compute Savings Plans once workload usage patterns stabilize.

---

## 3. Reliability Pillar

- **Design for Failure (Multi-AZ):** Always span critical workloads across at least two Availability Zones.
- **Use Auto Scaling Groups:** Allow EC2 clusters to self-heal by replacing failed instances automatically.
- **Enable RDS Multi-AZ:** Ensure automated database failover with synchronous standby replication.
- **Perform Regular Backup Drills:** Regularly test restoring databases and volumes from snapshots to validate recovery time objectives (RTO).

---

## 4. Performance Efficiency Pillar

- **Leverage Edge Caching:** Offload static assets and API caching to **Amazon CloudFront** to reduce origin server load.
- **Use In-Memory Caching:** Place **Amazon ElastiCache (Valkey / Redis)** in front of databases to accelerate frequent read queries.
- **Right-Size Resources:** Analyze **AWS Compute Optimizer** recommendations to avoid paying for over-provisioned CPU and memory.

---

## 5. Operational Excellence Pillar

- **Adopt Infrastructure as Code (IaC):** Define and manage all AWS resources declaratively using **AWS CDK (Cloud Development Kit)** or **Terraform** rather than manual console clicks.
- **Tag All Resources:** Establish a consistent tagging strategy (`Environment`, `Owner`, `Project`, `CostCenter`) for auditing and cost attribution.
- **Automate Observability:** Use CloudWatch Alarms and EventBridge rules to detect and remediate operational anomalies automatically.
- **Separate Accounts by Environment:** Use **AWS Organizations** to maintain isolated accounts for `development`, `staging`, and `production`.
