# AWS CLI Basics

The **AWS Command Line Interface (AWS CLI)** is an open-source tool that enables you to interact with and automate AWS services directly from your terminal.

---

## 1. Installation

=== "macOS (Homebrew)"
    ```bash
    brew install awscli
    ```

=== "Linux (x86_64)"
    ```bash
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
    ```

=== "Windows (MSI Installer)"
    Download and execute the official installer from:  
    `https://awscli.amazonaws.com/AWSCLIV2.msi`

Verify your installation:
```bash
aws --version
```

---

## 2. Configuration

### Option A: Standard Configuration (Access Keys)

```bash
aws configure
```

You will be prompted for:
1. **AWS Access Key ID:** `AKIAIOSFODNN7EXAMPLE`
2. **AWS Secret Access Key:** `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`
3. **Default region name:** `us-east-1` (or your preferred region)
4. **Default output format:** `json` (or `table` / `text`)

### Option B: Modern SSO Login (IAM Identity Center — Recommended)

```bash
# Configure SSO session once
aws configure sso

# Daily fast authentication via browser popup
aws sso login
```

---

## 3. Essential Everyday AWS CLI Commands

### Identity & Account
```bash
# Verify who you are authenticated as
aws sts get-caller-identity
```

### Amazon S3
```bash
# List all buckets in the account
aws s3 ls

# List contents of a specific bucket
aws s3 ls s3://my-bucket/

# Upload a file
aws s3 cp document.pdf s3://my-bucket/docs/

# Sync a local directory to S3
aws s3 sync ./build s3://my-bucket/website/

# Delete an object
aws s3 rm s3://my-bucket/docs/document.pdf
```

### Amazon EC2
```bash
# List all running instances
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"

# Start an instance
aws ec2 start-instances --instance-ids i-0123456789abcdef0

# Stop an instance
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
```

### AWS Lambda
```bash
# List all Lambda functions
aws lambda list-functions

# Invoke a function synchronously (AWS CLI v2 requires raw-in-base64-out for string payloads)
aws lambda invoke \
  --function-name processOrder \
  --cli-binary-format raw-in-base64-out \
  --payload '{"orderId": 123}' \
  response.json
```

---

## 4. Working with Multiple Profiles

Manage multiple AWS accounts (e.g., Development, Staging, Production) effortlessly using named profiles:

```bash
# Configure a named profile
aws configure --profile dev-account

# Execute a command against a specific profile
aws s3 ls --profile dev-account

# Set the active profile for your current terminal session
export AWS_PROFILE=dev-account
```
