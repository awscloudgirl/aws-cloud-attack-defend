# AWS IAM Enumeration & Role Assumption Guide

*A practical walkthrough using AWS CLI to enumerate IAM permissions, investigate roles, and access AWS resources.*

---

## Prerequisites

* AWS IAM credentials (Access Key ID and Secret Access Key)
* AWS CLI installed (e.g., via Kali or PwndCloudOS)
* Basic Linux command line familiarity

---

## 1. Configure AWS CLI

```bash
aws configure
```

Enter:

* Access Key ID
* Secret Access Key
* Region (e.g., `us-west-1`)
* Output format (`json`)

---

## 2. Verify Current Identity

```bash
aws sts get-caller-identity
```

Displays the user or role you're currently authenticated as.

---

## 3. Get IAM User Metadata

```bash
aws iam get-user
```

Returns metadata about the authenticated IAM user.

---

## 4. List User Group Memberships

```bash
aws iam list-groups-for-user --user-name <USERNAME>
```

Check if the user belongs to any IAM groups.

---

## 5. List Attached User Policies

```bash
aws iam list-attached-user-policies --user-name <USERNAME>
```

Shows any AWS-managed or customer-managed policies attached to the user.

---

## 6. List Inline User Policies

```bash
aws iam list-user-policies --user-name <USERNAME>
```

Inline policies are directly embedded in the user entity.

---

## 7. Investigate Managed Policies

```bash
aws iam list-policy-versions --policy-arn <POLICY_ARN>
```

```bash
aws iam get-policy-version --policy-arn <POLICY_ARN> --version-id <VERSION_ID>
```

View the document associated with managed policy versions.

---

## 8. Review Attached Role Policies

```bash
aws iam list-attached-role-policies --role-name <ROLE_NAME>
```

List all policies attached to a role.

---

## 9. Get Role Trust Policy

```bash
aws iam get-role --role-name <ROLE_NAME>
```

Shows which principals (users, roles) can assume this role.

---

## 10. Get Role Policy Details

```bash
aws iam get-policy --policy-arn <POLICY_ARN>
```

```bash
aws iam get-policy-version --policy-arn <POLICY_ARN> --version-id <VERSION_ID>
```

Retrieves detailed permissions for a policy.

---

## 11. Assume a Role

```bash
aws sts assume-role --role-arn <ROLE_ARN> --role-session-name <SESSION_NAME>
```

Returns temporary security credentials.

---

## 12. Export Temporary Credentials

```bash
export AWS_ACCESS_KEY_ID=<value>
export AWS_SECRET_ACCESS_KEY=<value>
export AWS_SESSION_TOKEN=<value>
```

These values allow you to act as the assumed role.

---

## 13. Confirm Role Assumption

```bash
aws sts get-caller-identity
```

Confirm you're operating as the new role.

---

## 14. List Secrets

```bash
aws secretsmanager list-secrets
```

Enumerate stored secrets (if allowed).

---

## 15. Retrieve a Secret

```bash
aws secretsmanager get-secret-value --secret-id <SECRET_NAME>
```

Fetches the value of a specific secret.

---

## 16. List S3 Bucket Contents

```bash
aws s3 ls s3://<BUCKET_NAME>
```

Check if you can list or access objects.

---

## 17. Download S3 File

```bash
aws s3 cp s3://<BUCKET_NAME>/<FILE_NAME> .
```

Copies an object locally.

---

## 18. Summary

This guide covered:

* IAM enumeration with AWS CLI
* Investigating IAM roles and policies
* Assuming roles and accessing secrets or S3 buckets

---

## Resources

* [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
* [PwnedLabs Cloud Attack & Defense Bootcamp](https://pwnedlabs.io)
* Tested from terminal environments like PwndCloudOS

---
