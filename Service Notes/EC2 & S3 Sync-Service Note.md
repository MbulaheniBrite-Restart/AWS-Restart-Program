# AWS Service Note: EBS Snapshots & S3 Sync Implementation

**Date:** 04-10-2025  
**Created by:**  Brite Sendedza  
**Environment:** AWS US West (Oregon) - us-west-2

---

## Overview

This service note documents my implementation of an automated backup and file synchronization solution using AWS EC2, EBS snapshots, and S3. The goal was to create a reliable data protection strategy that combines volume-level snapshots with object-level file versioning.

---

## AWS Services Used

### Amazon EC2 (Elastic Compute Cloud)
EC2 is basically AWS's virtual server service. I worked with two instances in this lab:
- **Command Host (t3.medium):** This was my main workstation where I ran all the CLI commands and scripts
- **Processor (t3.micro):** The instance I was backing up, attached to the EBS volume I needed to snapshot

EC2 gave me the flexibility to stop instances for consistent snapshots and attach IAM roles for secure service access.

### Amazon EBS (Elastic Block Store)
EBS provides the persistent storage volumes attached to my EC2 instances. Think of it like a hard drive for your virtual machines. The cool thing about EBS is that I could create point-in-time snapshots without losing data, which is perfect for backup and disaster recovery scenarios.

**Key volume:** vol-066c04b96bf8bb138 (attached to my Processor instance)

### Amazon S3 (Simple Storage Service)
S3 is AWS's object storage service - incredibly scalable and durable. I used it to store my synced files with versioning enabled, which meant I could recover deleted files or previous versions whenever needed.

**Bucket created:** s3-brite-bucket (General Purpose, multi-AZ)

### IAM (Identity and Access Management)
IAM is how I managed permissions securely. Instead of hardcoding credentials, I created a role called "S3BucketAccess" and attached it to my EC2 instance. This way, my instance could talk to S3 without me having to manage access keys.

### AWS CLI & Boto3
- **AWS CLI:** The command-line tool I used for everything from creating snapshots to syncing files
- **Boto3:** AWS's Python SDK that let me write automation scripts to handle snapshot creation and cleanup

---

## Implementation Steps

### 1. Initial Setup
I started by setting up my S3 bucket in the us-west-2 region and making sure my EC2 instances were ready to go. Both instances were running and passed their status checks.

### 2. IAM Role Configuration
Attached the S3BucketAccess IAM role to my Processor instance so it could interact with S3 securely. This follows AWS best practices by using instance roles instead of embedding credentials.

### 3. Manual Snapshot Creation
I connected to my Command Host using EC2 Instance Connect (browser-based SSH - super convenient) and ran through the manual snapshot process:
- Identified my volume and instance IDs
- Stopped the Processor instance for a consistent snapshot
- Created the snapshot using AWS CLI
- Waited for completion
- Restarted the instance

### 4. Snapshot Automation
Set up a cron job to automate the snapshot process. While the lab had me run it every minute for testing, in production I'd definitely space this out more (maybe daily or weekly depending on requirements).

### 5. Python Script for Snapshot Management
Wrote a Python script (snapshooter_v2.py) using boto3 that:
- Creates snapshots for all attached volumes
- Lists existing snapshots sorted by creation time
- Automatically deletes old snapshots, keeping only the 2 most recent ones (retention policy)

This prevents snapshot sprawl and keeps costs under control.

### 6. S3 File Synchronization
Moved on to syncing files between my EC2 instance and S3:
- Uploaded initial files (file1.txt, file2.txt, file3.txt)
- Used aws s3 sync with the --delete flag to mirror the local directory
- Enabled S3 versioning to track file changes and deletions

### 7. Testing Version Recovery
Tested S3's versioning feature by deleting a file locally, syncing to S3, and then viewing the version history. I could see both the previous version and the delete marker, which means recovery would be straightforward if needed.

---

## Challenges Faced

- **Snapshot timing consistency:** Initially wasn't sure if I needed to stop the instance for snapshots. Learned that stopping ensures data consistency, especially for databases or active workloads.

- **Understanding sync --delete behavior:** The first time I ran sync with --delete, I was surprised when file1.txt disappeared from S3. Realized it was because that file wasn't in the directory I was syncing from.

- **Cron job logging:** Had to figure out proper logging for my cron job since cron runs silently. Added output redirection to /tmp/cronlog so I could troubleshoot if something went wrong.

- **Snapshot retention logic:** Writing the Python script to properly sort and prune old snapshots took some trial and error. Had to make sure I was sorting by start_time correctly before deleting.

---

## Solutions Implemented

- **Consistent snapshots:** Always stop the instance before creating snapshots for critical workloads. For less critical systems, could use application-consistent snapshots or just accept the small risk.

- **Clear directory structure:** Organized my files properly and documented which directories sync to S3. Now I always double-check with ls before running sync --delete.

- **Robust logging:** Added proper logging to both cron jobs and Python scripts with timestamps. Makes debugging so much easier.

- **Tested retention policy:** Ran my Python script multiple times to verify it only kept the specified number of snapshots (MAX_SNAPSHOTS = 2). Also added safeguards so it never deletes ALL snapshots accidentally.

---

## Best Practices Applied

### Security
- Used IAM roles instead of access keys
- Applied principle of least privilege (S3BucketAccess only gives necessary S3 permissions)
- Connected to instances using EC2 Instance Connect (no SSH keys to manage)

### Cost Optimization
- Implemented snapshot retention policy to avoid unnecessary storage costs
- Used appropriate instance types (t3.micro for the workload being backed up)
- Only kept the most recent snapshots needed for recovery

### Reliability
- Stopped instances before snapshots to ensure data consistency
- Enabled S3 versioning for file-level recovery
- Automated the entire process to eliminate human error

### Operational Excellence
- Automated snapshot creation with cron
- Built self-managing cleanup scripts in Python
- Added logging for troubleshooting and audit trails
- Tested recovery procedures (S3 version listing)

---

## Key Takeaways

**What worked really well:**
- The combination of EBS snapshots (volume-level) and S3 sync (file-level) gives me flexibility in recovery options
- Python + boto3 made automation straightforward and maintainable
- IAM roles simplified security without compromising access

**What I'd do differently next time:**
- Set up SNS notifications for snapshot completion/failures
- Add more error handling in my Python script
- Consider using AWS Backup service for more complex scenarios
- Implement lifecycle policies on S3 to transition old versions to cheaper storage tiers

**Production considerations:**
- Snapshot frequency should match RPO (Recovery Point Objective) requirements
- Test restores regularly - backups are only good if you can actually recover from them
- Monitor snapshot costs as they accumulate over time
- Consider cross-region snapshot copies for disaster recovery

---

## Commands Reference

**Create snapshot manually:**

aws ec2 create-snapshot --volume-id vol-066c04b96bf8bb138 --description "Manual snapshot"
aws ec2 wait snapshot-completed --snapshot-ids snap-xxxxx

**Sync to S3:**

aws s3 sync /local/directory s3://s3-brite-bucket/files/
aws s3 sync /local/directory s3://s3-brite-bucket/files/ --delete

**List S3 versions:**

aws s3api list-object-versions --bucket s3-brite-bucket --prefix files/

**Describe snapshots:**

aws ec2 describe-snapshots --owner-ids self --filters "Name=volume-id,Values=vol-066c04b96bf8bb138"

---

## Conclusion

This lab gave me hands-on experience with core AWS backup and data protection services. I now have a solid understanding of how to implement automated snapshot management and file synchronization that could easily scale to production environments. The key is combining the right tools (EBS snapshots for volumes, S3 for files) with proper automation and retention policies.

**Status:** Implementation Complete  
**Next Steps:** Apply these patterns to production workloads, set up monitoring and alerting, document recovery procedures

---
