# Identify the AWS Account ID from a Public S3 Bucket

This documentation provides a step-by-step guide for identifying an AWS account ID by leveraging a public S3 bucket and using enumeration techniques. This guide is intended for ethical red team engagements and educational purposes.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Configure AWS CLI](#configure-aws-cli)
3. [Scan the IP Address](#scan-the-ip-address)
4. [Identify S3 Bucket](#identify-s3-bucket)
5. [Enumerate AWS Account ID](#enumerate-aws-account-id)
6. [Find the S3 Bucket Region](#find-the-s3-bucket-region)
7. [List Public EBS Snapshots](#list-public-ebs-snapshots)
8. [Troubleshooting](#troubleshooting)

---

## 1. Prerequisites

- **AWS CLI** installed on your system.
- **Python 3** installed.
- **Basic Linux command line knowledge**.
- **IAM credentials** for enumeration.

---

## 2. Configure AWS CLI

Set up the AWS CLI with the provided credentials. **Do not share your credentials publicly.**

```bash
aws configure
```

Enter the following placeholder values:

- **AWS Access Key ID**: `AKIAxxxxxxxxxxxxxxxx`
- **AWS Secret Access Key**: `UdUVhr+xxxxxxxxxxxxxxxxxxxxxxxxx`
- **Default region name**: `us-east-1`
- **Default output format**: `json`

---

## 3. Scan the IP Address

Use **Nmap** to scan the provided IP address for open ports:

```bash
nmap -Pn 54.204.171.32
```

Check if port 80 (HTTP) is open and visit the website in a browser.

---

## 4. Identify S3 Bucket

Inspect the website’s source code for references to S3 buckets. For example:

```
https://s3.amazonaws.com/mega-big-tech
```

---

## 5. Enumerate AWS Account ID

Use the `s3-account-search` tool to identify the AWS account ID associated with the public S3 bucket.

### Install the Tool

```bash
python3 -m pip install s3-account-search
```

Add the tool to your path:

#### For Bash

```bash
echo 'export PATH=$PATH:~/.local/bin' >> ~/.bashrc
source ~/.bashrc
```

#### For Zsh

```zsh
echo 'export PATH=$PATH:~/.local/bin' >> ~/.zshrc
source ~/.zshrc
~/.bashrc
```

### Run the Tool

```bash
s3-account-search arn:aws:iam::427648302155:role/LeakyBucket mega-big-tech
```

This command will output the AWS account ID, e.g., `107513503799`.

---

## 6. Find the S3 Bucket Region

Identify the region of the S3 bucket using **cURL**:

```bash
curl -I https://mega-big-tech.s3.amazonaws.com
```

Look for the `x-amz-bucket-region` header in the response, e.g., `us-east-1`.

---

## 7. List Public EBS Snapshots

Use the AWS account ID to search for public EBS snapshots in the identified region.

### AWS CLI Command to List Public EBS Snapshots

```bash
aws ec2 describe-snapshots --owner-ids 107513503799
```

This command lists all public EBS snapshots owned by the AWS account `107513503799`.

1. Log into the **AWS Management Console**.
2. Select the **EC2 service**.
3. In the left-hand menu, click **Snapshots** under **Elastic Block Store**.
4. Select **Public snapshots**.
5. Enter the AWS account ID (`107513503799`) in the search field.

![Snapshot](/images/Snapshot.png)

If any public EBS snapshots are found, you can explore them for sensitive data.

---

## 8. Troubleshooting

### Access Denied Errors

- Ensure your IAM credentials are correctly configured.
- Verify the role permissions allow S3 enumeration.

### Invalid Credentials

- Check your Access Key and Secret Key.
- Ensure the correct region is set.

### Tool Installation Issues

Refer to the [s3-account-search GitHub Repository](https://github.com/WeAreCloudar/s3-account-search) for installation instructions and troubleshooting.

---

## Further Reading

- [AWS: Limit Access to S3 Buckets by Account ID](https://aws.amazon.com/blogs/storage/limit-access-to-amazon-s3-buckets-owned-by-specific-aws-accounts/)
- [GitHub: s3-account-search](https://github.com/WeAreCloudar/s3-account-search)
- [ZeusCloud Blog: AWS Account ID - An Attacker's Perspective](https://www.zeuscloud.io/post/aws-account-id-an-attackers-perspective)

