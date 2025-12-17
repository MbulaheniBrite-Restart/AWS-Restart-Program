# Service Note: Setting Up Remote Access and Managing Users on AWS EC2

**Date:** 12-Decemebr-2025  
**Engineer:** Brite Sendedza  
**Service:** AWS EC2 Linux User Administration  
**Instance Type:** Amazon Linux 2

---

## 📝 What I Did

So I spent today working through a hands-on lab where I set up secure remote access to an EC2 instance and created a whole user management system from scratch. This note basically documents everything I did, what worked, what didn't, and what I learned along the way.

---

## 🎯 What I Was Trying to Accomplish

Going into this, I had a few main things I needed to get done:

1. Get PuTTY configured and connect securely to an EC2 instance via SSH
2. Create a bunch of user accounts for different departments
3. Set up groups so permissions would be easier to manage
4. Make sure the security was actually working (non-admin users can't do admin stuff)
5. Check the logs to see that everything was being tracked properly

---

## 🛠️ The AWS Services I Used

### Amazon EC2 (Elastic Compute Cloud)

So EC2 is basically just a virtual server in AWS's cloud. Instead of having a physical Linux box sitting somewhere, you just spin one up in the cloud and access it remotely. Pretty convenient for labs like this!

**How I used it:**
- This was my remote Linux playground where I could practice system admin tasks
- Gave me a safe environment to create users and mess with permissions without breaking anything important
- Let me practice SSH access and security configurations

**My instance details:**
- **IP Address:** 16.144.132.13
- **Operating System:** Amazon Linux 2 (heads up—it's being retired in June 2026)
- **How I connected:** SSH on port 22

### Amazon Linux 2

This is AWS's own flavor of Linux. It's optimized for running on EC2 and comes with a bunch of AWS tools already baked in. One thing I learned today though—it's getting old. AWS is pushing everyone to upgrade to Amazon Linux 2023, which has support until 2028. Good to know!

### PuTTY (My SSH Client)

Since I was working from Windows, I used PuTTY to connect to the EC2 instance. I set it up with:
- A private key file (`.ppk` format) instead of using passwords
- Keepalive settings so my connection wouldn't randomly drop
- Some TCP tweaks to make everything more responsive

---

## 🔧 Step-by-Step: What I Actually Did

### 1. Getting Connected via SSH

First thing was getting PuTTY talking to my EC2 instance. I plugged in the public IP address, loaded up my private key file, and made sure the connection settings were solid. The key-based authentication is honestly so much better than typing passwords—once it's set up, you just connect and you're in!

**What I configured:**
- Set keepalives to 30 seconds (learned this prevents the connection from timing out when I'm just reading stuff)
- Turned off Nagle's algorithm for snappier performance
- Verified the SSH fingerprint when connecting for the first time (security best practice!)

### 2. Creating All the User Accounts

Next up, I needed to create 10 different user accounts. This was pretty straightforward—just used `useradd` for each person and then set their passwords with `passwd`. Each user got their own home directory automatically.

**Here's who I added:**
- arosalez (Sales Manager)
- eowusu, jdoe, psantos (all in Shipping)
- ljuan (HR Manager)
- smartinez (HR Specialist)  
- mmajor (Finance Manager)
- ssarkar (Finance Specialist)
- mjackson (CEO)
- nwolf (Sales Rep)

Everyone started with the same temporary password (P@ssword1234!) which they'd change on first login.

### 3. Building Out the Group Structure

This is where things got interesting. Instead of managing permissions for each user individually, I created groups based on departments. Way more efficient!

**Groups I set up:**
- Department groups: Sales, HR, Finance, Shipping
- A Managers group for all the leadership folks
- CEO group (just for the big boss)
- Personnel group for general staff stuff

Then I assigned everyone to their groups using `usermod -aG`. The `-aG` part is important—it adds the group without removing any other groups they're already in. I learned that the hard way!

### 4. Testing the Security Setup

Okay, this was actually kind of fun. I wanted to make sure regular users couldn't do admin stuff, so I switched to one of the new accounts (arosalez) and tried to:
- Create a file in a protected directory → Permission denied! ✅
- Use sudo to try again → Got told "you're not in the sudoers file, and this will be reported" ✅

Then I checked `/var/log/secure` and yep, everything was logged. The system was definitely working as intended!

---

## 💡 Commands I Used Most

```bash
# Creating and managing users
sudo useradd <username>
sudo passwd <username>
sudo cat /etc/passwd | cut -d: -f1

# Working with groups
sudo groupadd <groupname>
sudo usermod -aG <groupname> <username>
getent group <groupname>

# Checking security stuff
su - <username>
sudo cat /var/log/secure
```

---

## ⚠️ Problems I Ran Into

- **Wrong key format** - I had an `.pem` key instead PuTTY needs `.ppk` files specifically
- **Trouble group assignment** - Used `-g` instead of `-aG` and accidentally wiped out the user's primary group (oops)
- **Username case sensitivity** - Created a user with the wrong case and had to delete and recreate it

---

## ✅ How I Fixed Everything

- **Converted the key** - I downloaded the correct `.ppk` format key
- **Made a note to use `-aG`** - Made a mental note to always use `-aG` to append groups, never just `-g`
- **Made a naming standard** - Decided all usernames would be lowercase to avoid confusion

---

## 📊 End Results

Got SSH working with key-based authentication (no passwords needed!)  
Successfully created all 10 user accounts with proper naming  
Built out the entire 7-group structure  
Confirmed that security boundaries are working—regular users can't sudo  
Verified that all activity is being logged in `/var/log/secure`

Everything's set up and ready to go. The user structure is solid, permissions are properly separated by department, and the security logging is capturing everything.

---

## 🎓 What I Learned Today

1. **Key-based auth is the way to go** - Once you get past the initial setup, it's actually way easier than passwords. Plus it's more secure.

2. **Plan your groups before creating users** - I'm glad I mapped this out first. Trying to reorganize groups after the fact would've been a pain.

3. **That `-aG` flag is crucial** - Using just `-G` replaces all supplementary groups, which is almost never what you want. The `a` stands for "append" and that's what you need.

4. **Security logs are actually useful** - I used to ignore `/var/log/secure` but it's really handy for troubleshooting and seeing what happened.

5. **Amazon Linux 2 is going away** - Need to remember this for future projects. Amazon Linux 2023 is where it's at now.

---

## 🔐 Security Stuff to Remember

- All users got temporary passwords that need to be changed
- Need to remind everyone to update their password on first login
- EC2 instance is only accessible with the SSH key (no password login)
- Root login via SSH is disabled
- Everything administrative gets logged to `/var/log/secure`

---

**Status:** Done  
**Sign-off:** Brite Sendedza  
**Date:** 12-Decemebr-2025
