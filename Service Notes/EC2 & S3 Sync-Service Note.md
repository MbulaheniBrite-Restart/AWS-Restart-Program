# AWS Service Note: EBS Snapshots & S3 Sync Implementation

**Date:** 04-10-2025  
**Created by:** Brite Sendedza  
**Environment:** AWS US West (Oregon) - us-west-2

---

## Overview

So I just wrapped up setting up an automated backup and file sync solution using AWS. The whole idea was to build a solid data protection strategy that covers both volume-level snapshots (with EBS) and file-level versioning (with S3). Pretty cool stuff, honestly.

---

## AWS Services I Got My Hands On

### Amazon EC2 (Elastic Compute Cloud)
EC2 is basically where you get your virtual servers on AWS. I had two instances running for this lab:
- **Command Host (t3.medium):** This was my main workstation where I ran all the CLI commands and wrote my scripts
- **Processor (t3.micro):** The instance I was actually backing up—this one had the EBS volume I needed to snapshot

What I really liked about EC2 here is that I could stop instances when I needed consistent snapshots, and I could attach IAM roles directly to them for secure access to other AWS services.

### Amazon EBS (Elastic Block Store)
Think of EBS as the hard drives for your virtual machines. These volumes stick around even if you stop or restart your instances, which is exactly what you want. The best part? I could take point-in-time snapshots without losing any data, which is perfect for backups and disaster recovery.

**The volume I worked with:** vol-066c04b96bf8bb138 (attached to my Processor instance)

### Amazon S3 (Simple Storage Service)
S3 is AWS's object storage—super scalable and crazy durable. I used it to store my synced files with versioning turned on, which means I can go back and grab old versions of files or even recover deleted ones. It's like having a time machine for your files.

**My bucket:** s3-brite-bucket (set up as General Purpose with multi-AZ for redundancy)

### IAM (Identity and Access Management)
IAM is how I kept things secure without making life difficult. Instead of dealing with access keys and secrets, I just created a role called "S3BucketAccess" and slapped it on my EC2 instance. Now my instance can talk to S3 without me having to worry about managing credentials.

### AWS CLI & Boto3
- **AWS CLI:** The command-line tool I used for pretty much everything—creating snapshots, syncing files, you name it
- **Boto3:** AWS's Python SDK that let me write scripts to automate the snapshot creation and cleanup process

---

## What I Actually Did

### 1. Getting Everything Set Up
First thing I did was create my S3 bucket in us-west-2 and make sure both my EC2 instances were up and running. Everything passed status checks, so I was good to go.

### 2. IAM Role Setup
I attached the S3BucketAccess IAM role to my Processor instance. This is way better than hardcoding credentials—follows AWS best practices and keeps things secure.

### 3. Creating Snapshots Manually (First Time)
I connected to my Command Host using EC2 Instance Connect (which is just SSH through your browser—super convenient) and went through the whole snapshot process manually:
- Found my volume ID and instance ID
- Stopped the Processor instance to get a clean snapshot
- Created the snapshot using AWS CLI
- Waited for it to finish
- Started the instance back up

### 4. Automating the Snapshots
Set up a cron job to handle snapshots automatically. The lab had me running it every minute for testing (which is pretty aggressive), but in real life, I'd probably do daily or weekly backups depending on how critical the data is.

### 5. Python Script for Managing Snapshots
Wrote a Python script called snapshooter_v2.py using boto3 that does three things:
- Creates snapshots for all attached volumes
- Lists existing snapshots sorted by when they were created
- Automatically deletes old snapshots, keeping only the 2 most recent (my retention policy)

This keeps snapshot costs from spiraling out of control, which is important.

### 6. Syncing Files to S3
Next up was getting files from my EC2 instance into S3:
- Uploaded some test files (file1.txt, file2.txt, file3.txt)
- Used aws s3 sync with the --delete flag to mirror what's on my instance
- Turned on S3 versioning so I could track changes and deletions

### 7. Testing the Version Recovery
I tested S3's versioning by deleting a file locally, syncing to S3, and then checking the version history. Sure enough, I could see both the old version and the delete marker. So if I ever accidentally delete something, I can get it back.

---

## Challenges I Ran Into

- **Figuring out snapshot timing:** At first, I wasn't sure if I really needed to stop the instance for snapshots. Turns out, stopping it ensures everything's consistent, especially if you're running databases or active applications.

- **The --delete flag surprised me:** First time I ran sync with --delete, file1.txt vanished from S3. I panicked for a second, then realized it was because that file wasn't in the directory I was syncing from. Lesson learned.

- **Getting cron to log properly:** Cron jobs run silently, which makes troubleshooting a pain. I had to add output redirection to /tmp/cronlog so I could actually see what was happening.

- **Snapshot retention logic:** Writing the Python script to sort and delete old snapshots correctly took some trial and error. Had to make sure I was sorting by start_time properly before deleting anything.

---

## How I Fixed Things

- **Consistent snapshots:** Now I always stop the instance before creating snapshots for important workloads. For less critical stuff, I might skip it and just accept the tiny risk.

- **Better directory management:** I organized my files properly and now I always double-check with ls before running sync --delete`. Can't be too careful.

- **Proper logging everywhere:** Added logging to both my cron jobs and Python scripts with timestamps. Makes debugging way easier when something goes wrong.

- **Tested the retention policy:** Ran my Python script multiple times to make sure it only kept the right number of snapshots (MAX_SNAPSHOTS = 2). Also added checks so it never accidentally deletes ALL snapshots.

---

## Best Practices I Followed

### Security Stuff
- Used IAM roles instead of access keys (way more secure)
- Followed least privilege—S3BucketAccess only has the S3 permissions it needs
- Connected using EC2 Instance Connect (no SSH keys to manage or lose)

### Keeping Costs Down
- Set up snapshot retention to avoid paying for old snapshots I don't need
- Used the right instance sizes (t3.micro is perfect for the workload)
- Only keep the most recent snapshots that I actually need for recovery

### Making Things Reliable
- Stopped instances before snapshots to keep data consistent
- Enabled S3 versioning so I can recover individual files
- Automated everything to eliminate human error (because let's be real, I'd forget)

### Operational Excellence
- Automated snapshot creation with cron
- Built scripts that clean up after themselves
- Added logging for troubleshooting and keeping audit trails
- Actually tested the recovery process (because backups are worthless if you can't restore)

---

## What I Learned

**Things that worked great:**
- Combining EBS snapshots (for whole volumes) with S3 sync (for individual files) gives me flexibility in how I recover data
- Python + boto3 made the automation pretty straightforward
- IAM roles made security easy without making things complicated

**What I'd do differently:**
- Set up SNS notifications so I know when snapshots succeed or fail
- Add more error handling to my Python script (right now it's pretty basic)
- Look into AWS Backup for more complex scenarios—it might simplify things
- Set up lifecycle policies on S3 to move old versions to cheaper storage

**If I was doing this in production:**
- Snapshot frequency should match how much data loss you can tolerate (RPO)
- Actually test restores regularly—backups only matter if you can recover from them
- Keep an eye on snapshot costs as they add up over time
- Consider copying snapshots to another region for disaster recovery

---

## Commands I Used (Quick Reference)

**Creating snapshots manually:**
```bash
aws ec2 create-snapshot --volume-id vol-066c04b96bf8bb138 --description "Manual snapshot"
aws ec2 wait snapshot-completed --snapshot-ids snap-xxxxx
```

**Syncing to S3:**

aws s3 sync /local/directory s3://s3-brite-bucket/files/
aws s3 sync /local/directory s3://s3-brite-bucket/files/ --delete


**Listing S3 versions:**

aws s3api list-object-versions --bucket s3-brite-bucket --prefix files/


**Checking snapshots:**

aws ec2 describe-snapshots --owner-ids self --filters "Name=volume-id,Values=vol-066c04b96bf8bb138"


---

## Wrapping Up

This lab gave me some solid hands-on time with AWS backup and data protection services. I feel pretty confident now about implementing automated snapshot management and file synchronization that could actually work in production. The key is using the right tool for the job (EBS snapshots for volumes, S3 for files) and automating it properly with good retention policies.

**Status:** All Done 
**What's Next:** Apply these patterns to real workloads, set up monitoring and alerts, write up recovery procedures

---
