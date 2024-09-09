# AWS Resource Reporting Script

## Overview

This project provides a Bash script that reports all AWS resources, including S3 buckets, EC2 instances, Lambda functions, and IAM users. The script is designed to run on an EC2 instance and create a daily report of these resources. It utilizes AWS CLI commands and is scheduled to run automatically using cron jobs.

## Step-by-Step Instructions

### 1. Configure AWS CLI

Run the following command to configure your AWS CLI with your access key and secret access key:

```bash
aws configure
```
* Enter your AWS Access Key ID.
* Enter your AWS Secret Access Key.
* Specify the default region name (e.g., `us-east-1`).
* Specify the default output format (e.g., `json`).

### 2. Create the Reporting Script

Create a new script file named `project.sh` and add the following content:

```bash
vim project.sh
```

Insert the following script into `project.sh`:

```bash
#!/bin/bash

# Author:- Sarvesh
# Date:- 7/09/2024
#
# This script will report all the AWS resources

set -x

# Lists S3 buckets
touch /home/ec2-user/daily_report
aws s3 ls >> daily_report

# List EC2 instances
aws ec2 describe-instances | jq '.Reservations[].Instances[] | {InstanceId: .InstanceId, InstanceType: .InstanceType}' >> daily_report

# List all Lambda Functions
aws lambda list-functions >> daily_report

# List all the IAM users
aws iam list-users >> daily_report
```

### 3. Install and Configure Cron

Execute the following commands to install and start the cron service:

```bash
sudo yum install cronie
sudo systemctl status crond
sudo systemctl start crond
```

### 4. Set Permissions for the Script

Make the `project.sh` script executable:

```bash
sudo chmod u+x project.sh
```

### 5. Schedule the Script with Cron

Open the crontab editor to schedule the script to run daily at 5:30 PM:

```bash
crontab -e
```

Add the following line to the crontab:

```
30 17 * * * /home/ec2-user/project.sh
```

### 6. Verify Cron Jobs

To verify that the cron job has been set up correctly, run:

```bash
cronie -l
```

## Complete Summary

The AWS Resource Reporting Script is a comprehensive solution for monitoring and managing your AWS infrastructure. By executing this Bash script on an EC2 instance, users can automatically generate a daily report at 5:30 PM (17:30) that includes vital information about their AWS resources, such as:

* **S3 Buckets**: A list of all S3 buckets in the account.
* **EC2 Instances**: Details of EC2 instances, including their IDs and types.
* **Lambda Functions**: A list of all deployed Lambda functions.
* **IAM Users**: Information about all IAM users in the account.

The generated report is saved in a file named `daily_report` located in the `/home/ec2-user/` directory. This automated reporting process helps users keep track of their AWS resources, enabling them to:

* Audit resource usage effectively.
* Identify unused or idle resources for cost optimization.
* Track changes in the AWS environment over time.
* Generate reports for compliance or billing purposes.

By scheduling the script to run daily at 5:30 PM using cron jobs, users can ensure that they always have up-to-date information about their AWS resources without manual intervention. This project is an essential tool for AWS administrators and developers looking to maintain visibility and control over their cloud infrastructure.

