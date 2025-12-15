# 🖥️ Remote Access and Linux managing Users and Groups
**A hands-on walkthrough of SSH configuration, EC2 connectivity, and Linux user administration**

Welcome! In this lab, I'm going to walk you through everything I did to set up remote SSH access to an Amazon Linux 2 EC2 instance and perform some essential user and group management tasks.

---

## 🔐 SSH Connection Setup

### Step 1: Configuring My PuTTY Session

First things first—I needed to set up PuTTY to connect to my remote server. Here's what I configured:

- **Host Name:** `16.144.132.13`
- **Port:** `22` (standard SSH port)
- **Connection Type:** SSH
- **Close Window:** Only on clean exit (prevents accidental closures)

This establishes the basic connection parameters for secure remote access.

<img width="371" height="356" alt="2 1" src="https://github.com/user-attachments/assets/a6b6c702-6691-480c-a143-4ee1eb2775d5" />

---

### Step 2: Fine-Tuning Connection Stability

Nobody likes dropped connections, so I adjusted some TCP settings to keep things stable:

- **Keepalives:** Set to 30 seconds (helps prevent idle timeouts)
- **TCP_NODELAY:** Enabled (Nagle's algorithm disabled for better responsiveness)
- **IP Version:** Auto-detect

**Pro tip:** These keepalive settings are a lifesaver when working on flaky network connections!

<img width="302" height="272" alt="2 2" src="https://github.com/user-attachments/assets/b10446a0-9e41-47b9-857e-b33f86792a99" />

---

### Step 3: Setting Up Key-Based Authentication

Instead of using passwords (which can be a security risk), I configured PuTTY to use a private key file for authentication:

- **Private Key Location:** `C:\Users\BritePC\Downloads\labsuser(2).ppk`

Key-based authentication is way more secure and convenient—no passwords to remember or type!

<img width="317" height="285" alt="2 3" src="https://github.com/user-attachments/assets/cace61ef-0b31-40f1-b6ff-9f32fb1f87a1" />

---

### Step 4: Verifying the Server's Identity

When connecting for the first time, PuTTY showed me the server's fingerprint. This is actually a good security feature—it helps you verify you're connecting to the right server and not some imposter.

**Fingerprint:** `ssh-ed25519 255 SHA256:gcdeKJwEV3Hg1jLg0huRHuuMdE8gWXvAQPlQlBhXYfo`

I clicked **Accept** to cache the key for future connections. Always verify this fingerprint matches what you expect before accepting!

<img width="413" height="304" alt="3" src="https://github.com/user-attachments/assets/45080dac-79e9-4c98-9ac0-03526d151bca" />

---

### Step 5: Successfully Connected! 🎉

And we're in! I logged in as `ec2-user` using the public key authentication. The system greeted me with some important info:

- **OS:** Amazon Linux 2
- **EOL Notice:** June 30, 2026
- **Recommendation:** Upgrade to Amazon Linux 2023 (supported until March 15, 2028)

<img width="497" height="287" alt="4" src="https://github.com/user-attachments/assets/f4d5dbde-182b-4f39-9ee7-c6142ef366ff" />

---

### Step 6: Checking My Current Directory

Just to make sure I'm in the right place, I ran a quick `pwd` command:
bash
pwd

**Output:** `/home/ec2-user`

Perfect! I'm in my home directory and ready to start working.

<img width="617" height="308" alt="5" src="https://github.com/user-attachments/assets/7aaad430-eb68-428a-8854-48b41b88e5b5" />

---

## 👥 User Account Creation

### Step 7: Understanding the Requirements

For this part, I needed to create several user accounts for different departments. Here's the roster I was working with:

#### 📊 User Directory

| First Name | Last Name | Username  | Department           | Initial Password  |
|------------|-----------|-----------|----------------------|-------------------|
| Alejandro  | Rosalez   | arosalez  | Sales Manager        | P@ssword1234!     |
| Efua       | Owusu     | eowusu    | Shipping             | P@ssword1234!     |
| Jane       | Doe       | jdoe      | Shipping             | P@ssword1234!     |
| Li         | Juan      | ljuan     | HR Manager           | P@ssword1234!     |
| Mary       | Major     | mmajor    | Finance Manager      | P@ssword1234!     |
| Mateo      | Jackson   | mjackson  | CEO                  | P@ssword1234!     |
| Nikki      | Wolf      | nwolf     | Sales Representative | P@ssword1234!     |
| Paulo      | Santos    | psantos   | Shipping             | P@ssword1234!     |
| Sofia      | Martinez  | smartinez | HR Specialist        | P@ssword1234!     |
| Saanvi     | Sarkar    | ssarkar   | Finance Specialist   | P@ssword1234!     |

<img width="450" height="180" alt="6" src="https://github.com/user-attachments/assets/abab30e2-093e-41c5-a2a3-4e871a0886ac" />

---

### Step 8: Creating the User Accounts

Time to actually create these users! For each person, I ran two commands:
bash
### Create the user account
sudo useradd <username>

### Set their initial password
sudo passwd <username>


Then I verified all the users were created by checking `/etc/passwd`:
bash
sudo cat /etc/passwd | cut -d: -f1


This gave me a nice list of all usernames on the system. Seeing all the new accounts appear was pretty satisfying!

<img width="664" height="419" alt="7" src="https://github.com/user-attachments/assets/fbaf3122-bcf2-4c01-be89-cf941651001a" />

---

## 🗂️ Group Management

### Step 9: Creating Organizational Groups

Next up, I needed to create groups that align with our organizational structure. This makes managing permissions across departments much easier. Here are the groups I created:

- **Sales** - For the sales team
- **HR** - Human Resources folks  
- **Finance** - Financial department
- **Personnel** - General staff group
- **CEO** - Executive level access
- **Shipping** - Warehouse and logistics
- **Managers** - All management roles
  bash
sudo groupadd <groupname>
  

<img width="647" height="488" alt="8" src="https://github.com/user-attachments/assets/f0bcc1e0-007e-4c21-b27c-b356317910f0" />

---

### Step 10: Verifying Group Creation

I double-checked that all groups were created properly by looking at `/etc/group`:
bash
cat /etc/group


Each group got assigned a unique GID (Group ID). Here's what mine looked like:

- `Sales:x:1011:`
- `HR:x:1012:`
- `Finance:x:1013:`
- `Shipping:x:1014:`
- `Managers:x:1015:`
- `CEO:x:1016:`

<img width="1316" height="277" alt="9" src="https://github.com/user-attachments/assets/6b5ec22e-72a6-4483-98c4-8c1206857d3a" />

---

### Step 11: Assigning Users to Their Groups

Now for the fun part—putting everyone in their proper groups! This is where organization really matters.

#### 🔗 Final Group Assignments

| Group     | Members                                  |
|-----------|------------------------------------------|
| Sales     | arosalez, ec2-user                       |
| HR        | ljuan, smartinez, ec2-user               |
| Finance   | mmajor, ssarkar, ec2-user                |
| Shipping  | eowusu, jdoe, psantos, ec2-user          |
| Managers  | arosalez, ljuan, mmajor, ec2-user        |
| CEO       | mjackson, ec2-user                       |

I used this command to add users to groups:

bash
sudo usermod -aG <groupname> <username>


And verified memberships with:
bash
getent group <groupname>

<img width="607" height="165" alt="10" src="https://github.com/user-attachments/assets/01d1654d-393e-47f5-ac25-288c86161585" />

---

## 🔒 Security & Permissions

### Step 12: Understanding Sudoers

Here's something important I learned: not every user can run commands as root. Only **sudoers** (users listed in `/etc/sudoers`) have that privilege.

**Key points:**
- Sudoers can execute administrative commands using `sudo`
- All sudo activity gets logged to `/var/log/secure`
- This logging is crucial for security audits!

<img width="877" height="402" alt="11" src="https://github.com/user-attachments/assets/a84dd61a-187f-43b1-95b7-e3c9f56639b5" />

---

### Step 13: Testing Permission Boundaries

I wanted to see what happens when a regular user (not a sudoer) tries to do something they shouldn't. So I switched to the `arosalez` account and ran some tests:

bash
su - arosalez
touch myFile.txt


**Result:** Permission denied! ❌

Then I tried with sudo:
bash
sudo touch myFile.txt


**Result:** "arosalez is not in the sudoers file. This incident will be reported." 🚨

This is exactly what should happen—the system is protecting itself from unauthorized actions.

<img width="1302" height="92" alt="12" src="https://github.com/user-attachments/assets/f849c119-38c3-46e6-bc36-c65180274283" />

---

### Step 14: Reviewing Security Logs

Finally, I checked out the security log to see all the activity I'd generated:

bash
sudo cat /var/log/secure


The log captured everything:
- User account creations
- Password changes
- SSH login events
- Sudo command attempts (including the failed one!)

This is my audit trail—super important for security monitoring.

<img width="978" height="397" alt="13" src="https://github.com/user-attachments/assets/c6c36ec0-d099-4258-96f5-879b0179a33d" />
<br></br>
<img width="1883" height="940" alt="14" src="https://github.com/user-attachments/assets/83feea3e-6649-4f17-9375-c9ed660ae34e" />

---

## ✅ Lab Complete!

And that's a wrap! I successfully completed the entire lab, covering:

- SSH configuration and secure authentication
- User account provisioning
- Group management and organization
- Security boundaries and logging

The platform confirmed my completion—time to celebrate! 🎉

<img width="1186" height="113" alt="15" src="https://github.com/user-attachments/assets/5422f61e-ebb3-4030-9773-079a250c35f4" />

---

## 🛠️ Command Reference Quick Sheet

Here's a handy cheat sheet of all the commands I used throughout this lab:

### User Management
```bash
# Create a new user
sudo useradd <username>

# Set/change user password
sudo passwd <username>

# List all users on the system
sudo cat /etc/passwd | cut -d: -f1

# Switch to another user
su - <username>
```

### Group Management
```bash
# Create a new group
sudo groupadd <groupname>

# Add user to a group (append, don't replace)
sudo usermod -aG <groupname> <username>

# View all members of a group
getent group <groupname>

# List all groups
cat /etc/group
```

### System & Security
```bash
# Check current directory
pwd

# View security logs
sudo cat /var/log/secure

# Check your sudo privileges
sudo -l
```

---

## 📚 What I Learned

This lab gave me solid hands-on experience with:

1. **SSH best practices** - Key-based auth is the way to go!
2. **Linux user administration** - Creating and managing multiple accounts efficiently
3. **Group-based permissions** - Organizing users by department makes life easier
4. **Security awareness** - Understanding logs and permission boundaries

---
