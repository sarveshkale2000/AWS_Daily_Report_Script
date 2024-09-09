# AWS Resource Reporting Script

## Overview

This project provides a Bash script that reports all AWS resources, including S3 buckets, EC2 instances, Lambda functions, and IAM users. The script is designed to run on an EC2 instance and create a daily report of these resources. It utilizes AWS CLI commands and is scheduled to run automatically using cron jobs.

## Prerequisites
* An AWS account with appropriate permissions to access S3, EC2, Lambda, and IAM resources.
* An EC2 instance running Amazon Linux or a compatible distribution.
* AWS CLI installed and configured on the EC2 instance.

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

### Summary

By following these steps, you will have a Bash script that reports all your AWS resources and is scheduled to run daily on your EC2 instance. The output will be saved in a file named `daily_report` located in the `/home/ec2-user/` directory.
```
