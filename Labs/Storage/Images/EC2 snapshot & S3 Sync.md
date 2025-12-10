# My AWS Snapshots, S3 Sync, and Versioning Lab Journey

Hey there! Let me walk you through my adventure with AWS - covering everything from setting up S3 buckets to creating EBS snapshots and getting files synced with versioning. It's been a really cool hands-on experience! ☁️🪣💻

---

## Quick Overview of What I Did

| # | What I Did | Main Action | AWS Service |
|---|-------|------------|-------------|
| 1 | Mapped out the architecture | Understanding the snapshot + S3 sync workflow | EC2, EBS, S3 |
| 2 | Set my lab goals | Figured out what I needed to accomplish | EC2, EBS, S3 |
| 3 | Got into S3 | Started exploring S3 buckets | S3 |
| 4 | Built my bucket | Set up and configured my S3 bucket | S3 |
| 5 | Confirmed it worked | Made sure my bucket was ready to go | S3 |
| 6 | Checked my EC2 instances | Found the instances I'd be working with | EC2 |
| 7 | Added IAM permissions | Gave my EC2 instance access to S3 | IAM, EC2 |
| 8 | SSH'd into my instance | Connected through the browser | EC2 |
| 9 | Created snapshots manually | Stopped, snapshotted, and restarted my instance | EC2, EBS |
| 10 | Automated the process | Set up a cron job and verified it worked | EC2, EBS |
| 11 | Wrote a Python script | Built something to create and clean up snapshots | EC2 (boto3) |
| 12 | Cleaned up old snapshots | Deleted the old ones, kept the recent ones | EC2, EBS |
| 13 | Moved to S3 sync | Shifted gears to file synchronization | S3 |
| 14 | Synced and versioned files | Uploaded files, deleted some, explored versions | S3 |
| 15 | Double-checked my local files | Made sure I knew what was where | Shell |
| 16 | Wrapped it all up | Lab complete! | — |

---

## My Step-by-Step Walkthrough

### Screenshot 1: Understanding the AWS Architecture
First thing I did was look at the overall architecture diagram. It shows how everything connects - my Command Host and Processor living in a public subnet, hooked up to an EBS volume that I'll be snapshotting and syncing to S3. The goal here was clear: automate snapshots using AWS CLI and eventually set up a Python script to prune old ones. Plus there's a challenge to tackle S3 sync.  
- **What caught my eye:** The EBS → Snapshot → S3 workflow and how CLI fits into everything.  
- **All the pieces:** Region, VPC, Availability Zone, Public subnet, EC2 roles, EBS, and S3.  
- **The flow:** Command Host talks to Processor, which connects to EBS, creates snapshots, and syncs to S3.  

<img width="1411" height="780" alt="A" src="https://github.com/user-attachments/assets/5e998642-dc9c-4904-9357-dd22b898a596" />

---

### Screenshot 2: What I Needed to Accomplish
Before diving in, I made sure I understood my objectives. I needed to get EBS snapshots working reliably, sync a directory to S3, and learn how to recover deleted files using S3's versioning feature.
- **Goal 1:** Get comfortable creating and managing EC2/EBS snapshots.  
- **Goal 2:** Use the S3 sync command to move files from EBS to S3.  
- **Goal 3:** Practice recovering deleted files with S3 versioning.  

<img width="1172" height="248" alt="B" src="https://github.com/user-attachments/assets/d0e0adae-cb67-4870-9694-ddc9a87b3109" />

---

### Screenshot 3: My First Look at Amazon S3
When I opened the S3 console, it gave me a nice intro to buckets (basically containers for your files). There were quick links to create a bucket, check pricing, and dive into documentation.
- **What I could do:** Create a bucket, look at pricing, read through guides and FAQs.  
- **Why S3 rocks:** It's scalable, highly available, secure, and performs really well.  
- **Helpful stuff:** There's even a "How it works" video if you're new to this.  

<img width="1820" height="817" alt="C" src="https://github.com/user-attachments/assets/24ea711b-e0a8-4d52-a32e-33d72055f7b2" />

---

### Screenshot 4: Creating My First Bucket
Time to actually create a bucket! I filled out the form, picked my region (US West Oregon), and chose a general-purpose bucket type for that multi-AZ resilience.
- **Region I picked:** US West (Oregon) - us-west-2.  
- **Bucket type:** General purpose (gives me multi-AZ storage options).  
- **Naming rules I had to follow:** 3-63 characters, globally unique, only lowercase letters, numbers, dots, and hyphens.  

<img width="1867" height="541" alt="D" src="https://github.com/user-attachments/assets/e2280f85-3ca8-4a3d-8f18-a43e47ae5e38" />

---

### Screenshot 5: Success! Bucket Created
And we're live! My bucket "s3-brite-bucket" was successfully created. The dashboard showed me options like Copy ARN, Empty, Delete, and of course Create another bucket.
- **My bucket:** s3-brite-bucket in us-west-2.  
- **Status:** Created successfully with a timestamp to prove it.  
- **Nice extras:** Got some account insights and external access summary panels.  

<img width="1867" height="697" alt="E" src="https://github.com/user-attachments/assets/c074ca4f-e6bf-409d-afd2-6f9b3a17112d" />

---

### Screenshot 6: Checking Out My EC2 Instances
I pulled up my EC2 dashboard and found two instances waiting for me: the Processor (t3.micro) and Command Host (t3.medium). Both were running in us-west-2a and all checks passed. I also noticed the security actions menu.  
- **My instances:** Processor (which I selected) and Command Host.  
- **What I could do:** Change security groups, modify the IAM role.  
- **Details visible:** Instance IDs, current state, availability zone, public DNS.  

<img width="1915" height="547" alt="F" src="https://github.com/user-attachments/assets/75f2a687-b442-4332-ac96-26a1363912b7" />

---

### Screenshot 7: Giving My Instance S3 Access
Here's where I attached an IAM role called "S3BucketAccess" to my Processor instance. This lets my EC2 instance talk to S3 securely without hardcoding any credentials. Smart!
- **Instance I modified:** i-0430dbe3f6de174aa (Processor).  
- **Role I attached:** S3BucketAccess.  
- **Why this matters:** It follows the principle of least privilege - my instance only gets the permissions it needs.  

<img width="1841" height="495" alt="G" src="https://github.com/user-attachments/assets/314e4288-d28f-437c-b736-88f4f5fa9b78" />

---

### Screenshot 8: Connecting via Browser SSH
Instead of using a local SSH client, I used EC2 Instance Connect to get into my Command Host right from the browser. Just needed the public IP and username (ec2-user). Super convenient!
- **Connection method:** EC2 Instance Connect using the public IP.  
- **My instance:** ID i-01c7dc7ae796742d8 with IP 54.191.134.28.  
- **Username:** ec2-user (this is pretty standard for Amazon Linux instances).  

<img width="1817" height="796" alt="H" src="https://github.com/user-attachments/assets/34c43ab5-f0bf-4f37-a98e-8351eceff2bd" />

---

### Screenshot 9: Creating My First Snapshot Manually
Now for the fun part! I used AWS CLI commands to grab my volume and instance IDs, stopped the instance, created a snapshot, waited for it to finish, then restarted everything. 📸  
- **What I identified:** VolumeId (vol-066c04b96bf8bb138) and InstanceId (i-0430dbe3f6de174aa).  
- **The process:** Stop instance → create snapshot → wait for it to complete → start instance back up.  
- **Result:** My snapshot went from pending to completed. Success!  

<img width="1728" height="822" alt="I" src="https://github.com/user-attachments/assets/a84da1a4-71e4-4d58-b458-6f4940568a96" />

---

### Screenshot 10: Automating Snapshots with Cron
I wasn't about to manually create snapshots forever, so I set up a cron job to do it automatically. Then I used the describe-snapshots command to verify everything was working and check the metadata. ⏱️  
- **My cron setup:** Configured it to run every minute (just for testing) and log to /tmp/cronlog.  
- **Verification:** The describe-snapshots output showed 100% progress with timestamps.  
- **State changes:** Watched my instance go stopped → pending → running after the start command.  

<img width="1512" height="782" alt="J" src="https://github.com/user-attachments/assets/d53c9379-27e2-4aa2-ac2f-6dc16f963d4d" />

---

### Screenshot 11: Leveling Up with a Python Script
I wrote a Python script (snapshooter_v2.py) using boto3 that creates snapshots for all my volumes and automatically prunes the old ones. I set it to keep only the 2 most recent snapshots. 
- **My retention rule:** MAX_SNAPSHOTS = 2 (keeps things tidy).  
- **How it works:** Loops through volumes → creates snapshots → lists existing snapshots → prunes the old ones.  
- **The cleanup logic:** Sorts by start time, then deletes anything beyond my retention cap.  

<img width="1168" height="472" alt="K" src="https://github.com/user-attachments/assets/fc6065ee-ff3d-4293-8188-0081b9651846" />

---

### Screenshot 12: Watching My Script Work
When I ran the script, I could see it deleting old snapshots in real-time. Then I used the CLI to confirm only the two most recent snapshot IDs remained. Exactly what I wanted!  
- **Ran it with:** python3.8 snapshooter_v2.py.  
- **Output:** Got log messages like "Deleting snapshot..." for the older ones.  
- **Verification:** describe-snapshots confirmed only my two latest snapshots were still there.  

<img width="1416" height="645" alt="L" src="https://github.com/user-attachments/assets/262125d5-7421-41d1-be90-3c5bca524321" />

---

### Screenshot 13: Shifting Gears to S3 Sync
Okay, snapshots done. Time to move on to synchronizing files with S3! This slide marked my transition from EBS snapshots to exploring the S3 sync command.
- **New topic:** Syncing a local directory to my S3 bucket.  
- **Why it matters:** Complements my snapshot backups with object-level versioning and easier file management.  
- **What's next:** Time to upload some files, sync them, delete stuff, and explore version history.  

<img width="626" height="46" alt="M" src="https://github.com/user-attachments/assets/48bfe1dc-c1af-476c-8a50-e42be5d2645c" />

---

### Screenshot 14: Syncing, Deleting, and Exploring Versions
Here's where things got interesting! I uploaded three files, then ran sync with the "--delete" flag which removed objects that didn't exist locally anymore. Then I checked out S3's versioning to see previous versions and delete markers.  
- **My upload:** file1.txt, file2.txt, file3.txt all went to s3://s3-brite-bucket/files/.  
- **The sync command:** aws s3 sync files s3://s3-brite-bucket/files/ --delete (which removed file1.txt from S3).  
- **Versioning magic:** list-object-versions showed me both the deleted version and the delete marker.  

<img width="1850" height="737" alt="O" src="https://github.com/user-attachments/assets/cde6b5d5-83eb-48c9-9ffd-9ead76b3e3c8" />

---

### Screenshot 15: Checking What I Had Locally
I ran some quick shell commands to see what files were actually in my local directory before and after the sync operations. This helped me understand why S3 did what it did.  
- **What I found:** ls files showed me file2.txt and file3.txt.  
- **Interesting detail:** ls file1.txt confirmed it existed but outside the files/ directory.  
- **Aha moment:** This location difference explained why the sync command deleted file1.txt from S3!  

<img width="837" height="87" alt="P" src="https://github.com/user-attachments/assets/a0db2d08-1655-4479-bfad-71911d09a9c1" />

---

### Screenshot 16: Lab Complete!
And that's a wrap! Got a nice confirmation message that I'd successfully completed all the lab activities and objectives. Feels good! 
- **Status:** Lab finished successfully.  
- **What I learned:** EBS snapshots, automation with Python and cron, S3 sync operations, and versioning.  
- **Next steps:** Time to apply this knowledge to real-world scenarios!  

<img width="747" height="112" alt="R" src="https://github.com/user-attachments/assets/826dfc25-9fe1-4b6c-8915-cd184b51c79a" />

---
