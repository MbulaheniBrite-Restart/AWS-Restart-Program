# My AWS EBS Volumes, Snapshots, and Recovery Lab Walkthrough

Let me walk you through my AWS lab on EBS volumes and snapshots;

---

## Quick Overview

Here's what I covered in this lab:

| # | What I Did | What's In The Screenshot | AWS Stuff Used |
|---|------|-------------------|-------------|
| 1 | Looked at the EBS flow | How EC2 connects to EBS and creates snapshots | EC2, EBS |
| 2 | Checked my instance | Made sure my EC2 was running properly | EC2 |
| 3 | Viewed my volumes | Saw what volumes I already had | EBS |
| 4 | Created a new volume | Set up a fresh 1 GiB volume | EBS |
| 5 | Confirmed it worked | Saw my new volume in the list | EBS |
| 6 | Attached it to EC2 | Connected the volume to my instance | EC2, EBS |
| 7 | Verified attachment | Both volumes showing as in-use | EC2, EBS |
| 8 | SSHed into the instance | Connected via EC2 Instance Connect | EC2 |
| 9 | Formatted the disk | Ran mkfs to create a filesystem | Linux |
| 10 | Created a snapshot | Backed up my volume | EBS |
| 11 | Checked snapshot status | Made sure it completed successfully | EBS |
| 12 | Restored from snapshot | Created a new volume from my backup | EBS |
| 13 | Attached restored volume | Mounted it as a second data disk | EC2, EBS |
| 14 | Tested the recovery | Verified my file was actually recovered | Linux |
| 15 | Wrapped it up | Lab completion screen | — |

---

## Step-by-Step: What I Actually Did

### 1: Understanding the EBS workflow
Before I started messing with anything, I took a look at how this whole thing works. Basically, you've got your **EBS volume** attached to your **EC2 instance**, and you can create **snapshots** from that volume. Pretty straightforward—this is the backup and restore workflow I'd be testing out.

<img width="1355" height="650" alt="1" src="https://github.com/user-attachments/assets/6300d261-b7c1-4496-8978-544b31ae99b9" />

---

### 2: My EC2 instance details
First thing I did was check on my instance. I had a **t3.micro** instance called "Lab" running in **us-west-2a**. Everything looked good—all 3 status checks passed, and I could see the public and private IPs. This meant I was ready to start playing with storage.

<img width="1847" height="752" alt="2" src="https://github.com/user-attachments/assets/2bb0bc2c-e237-44da-8004-17f25d04dac1" />

---

### 3: Checking out my existing volumes
I went to the **Volumes** section and saw I already had one **8 GiB gp2** volume (probably the root volume). There was also this notification saying **0/1** volumes were backed up recently, which was basically AWS telling me "hey, maybe you should set up some snapshots."

<img width="1452" height="685" alt="3" src="https://github.com/user-attachments/assets/80302302-bbe3-437d-b23d-bc3b427114be" />

---

### 4: Creating a brand new volume
Time to create a data volume! I went with **General Purpose SSD (gp2)**, kept it small at **1 GiB**, and made absolutely sure it was in **us-west-2a**—same availability zone as my instance. That's important because you can't attach a volume from a different AZ.

<img width="1237" height="767" alt="4" src="https://github.com/user-attachments/assets/d6e6e7a0-c268-410b-80e8-bb37dd46515e" />

---

### 5: Volume created successfully
The console showed me my shiny new volume: **vol-097bc07ece2143c3e**. I tagged it "My Volume" so I wouldn't lose track of it. Now I just needed to attach it to my instance.

<img width="1453" height="392" alt="5" src="https://github.com/user-attachments/assets/b5074ab9-a6e2-4c74-a929-92b10d7accc7" />

---

### 6: Attaching the volume
I selected my instance and chose **/dev/sdb** as the device name. There's this note about how Linux might rename it to something like **/dev/xvdf**, but I stuck with the default. As long as the AZ matches (which it did), we're golden.

<img width="1455" height="757" alt="6" src="https://github.com/user-attachments/assets/331725a8-02e6-43bc-b060-7489ec260a05" />

---

### 7: Both volumes attached and ready
Perfect! Now I could see both volumes listed as **In-use**—my root volume at **/dev/xvda** and my new data volume at **/dev/sdb**. The instance could now see the disk, so it was time to hop onto the command line.

<img width="1552" height="713" alt="7" src="https://github.com/user-attachments/assets/b523d386-b40a-45d2-867d-cb0037222c5f" />

---

### 8: Connecting to my instance
I used **EC2 Instance Connect** to SSH into the instance. Just clicked connect, used the default **ec2-user**, and boom—I was in. Now I could run the actual Linux commands to prep the disk.

<img width="1663" height="752" alt="8" src="https://github.com/user-attachments/assets/d7185795-ce3e-496b-8237-09c3972125b8" />

---

### 9: Formatting the volume
Once I was logged in, I needed to create a filesystem on the new volume. I ran:

sudo mkfs -t ext3 /dev/sdb

This formatted it as **ext3** and showed me all the technical details—block groups, inodes, all that good stuff. Now the volume was ready to actually use.

<img width="1917" height="645" alt="9" src="https://github.com/user-attachments/assets/36b2d7bc-3cc3-4beb-ad83-6f104300ac9f" />

---

### 10: Taking a snapshot
Back in the AWS console, I went to create a snapshot of **vol-097bc07ece2143c3e**. I gave it a name tag ("My Snapshot") because past me has learned that future me really appreciates good naming conventions. Then I hit create.

<img width="1577" height="765" alt="10" src="https://github.com/user-attachments/assets/ed837f8e-ff5a-412b-a895-530c76befa66" />

---

### 11: Snapshot completed
The snapshot **snap-0f8cdf4d362317961** finished at **100%** with a size of **53 MiB**. From here, I had options—I could copy it, delete it, create a volume from it, or even make an AMI. For this lab, I was going to restore a volume from it.

<img width="1145" height="677" alt="11" src="https://github.com/user-attachments/assets/35c77e1d-f5ea-40f8-902a-f160ce37d179" />

---

### 12: Creating a volume from the snapshot
This is where the magic happens. I used my snapshot to create a whole new volume (**vol-0f35dc7ea140bd75c**). This is basically the restore process—if I ever lost data on the original volume, I could spin up a new one from this snapshot and get everything back.

<img width="1146" height="516" alt="12" src="https://github.com/user-attachments/assets/17cb8b67-cc29-468e-9630-fccbf88c558e" />

---

### 13: Attaching the restored volume
I attached the new volume to my instance as **/dev/sdc**. Now I had three volumes attached—the root, the original data volume, and this restored one. Having multiple data volumes like this is totally normal when you're doing recovery stuff.

<img width="1447" height="722" alt="13" src="https://github.com/user-attachments/assets/4eee3a8b-ef66-44a9-b094-25e3ce37aeea" />

---

### 14: The moment of truth—verifying the recovery
Here's where I proved that the snapshot actually worked. I did a little test:

1. First, I deleted a test file: sudo rm /mnt/data-store/file.txt
2. Created a new mount point: sudo mkdir /mnt/data-store2
3. Mounted the restored volume: sudo mount /dev/sdc /mnt/data-store2
4. Checked for the file: ls /mnt/data-store2/file.txt

And there it was! The file existed on the restored volume. That's proof that the snapshot worked and I could recover my data. Success!

<img width="1263" height="542" alt="14" src="https://github.com/user-attachments/assets/49a01b34-4e01-4085-a1b1-c1143c0a7758" />

---

### 15: Lab complete!
And that's a wrap! The lab showed me the completion message. I successfully created volumes, attached them, took snapshots, restored from those snapshots, and verified the whole recovery process worked. Pretty cool stuff!

<img width="747" height="112" alt="15" src="https://github.com/user-attachments/assets/91dae68a-642a-4c7a-ac6b-de9d26409190" />

---

## Things I Learned

- **AZ matters:** You can't attach volumes to instances in different availability zones, so always double-check that.
- **Tag everything:** Seriously, name your volumes and snapshots. Future you will thank present you.
- **Format before using:** New volumes need `mkfs` before you can actually store anything on them.
- **Recovery is simple:** Just create volume from snapshot, attach it, mount it, and verify your data is there.
- **Security best practice:** Use IAM roles on instances instead of hardcoded credentials whenever possible.

---
