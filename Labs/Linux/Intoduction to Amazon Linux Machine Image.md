## 🖥️ Remote Session Lab: Connecting to Amazon Linux 2 with PuTTY

 **A hands-on walkthrough of setting up SSH connections, optimizing PuTTY settings, and navigating Amazon Linux 2**

Hey there! Welcome to my lab documentation. In this guide, I'll walk you through how I set up a remote SSH session using PuTTY to connect to an Amazon Linux 2 EC2 instance. Whether you're just getting started with remote server management or brushing up on your SSH skills, I've broken everything down step-by-step.

---

## 📋 What You'll Learn

- Setting up PuTTY for SSH connections
- Optimizing connection settings for better performance
- Verifying SSH host keys (security matters!)
- Logging into an EC2 instance running Amazon Linux 2
- Using Linux manual pages to find command documentation

---

## 🚀 Let's Get Started

### **Step 1: Setting Up the PuTTY Session**

First things first—I needed to configure PuTTY to connect to my remote server. Here's what I set up:

**Connection Details:**
- **Host Name:** 16.144.132.13
- **Port:** 22 (standard SSH port)
- **Connection Type:** SSH

I also made sure to set the window behavior to *"Close only on clean exit"* so the terminal doesn't disappear if something goes wrong—super helpful for troubleshooting!

<img width="647" height="205" alt="1" src="https://github.com/user-attachments/assets/26ecb348-dbcc-4582-a30b-20fd92f4cb6b" />

---

### **Step 2: Tweaking Connection Settings for Better Performance**

Okay, so here's where things get a bit more interesting. I dove into PuTTY's connection settings to optimize the session. These little tweaks make a noticeable difference in responsiveness:

**What I Changed:**
- **Keepalives:** Set to 30 seconds—this keeps the connection alive even during idle periods
- **TCP_NODELAY:** Enabled—disables Nagle's algorithm to reduce latency (translation: less lag!)
- **IP Version:** Left on Auto so PuTTY picks the best option

These settings help prevent timeouts and make the terminal feel snappier when you're typing commands.

<img width="371" height="356" alt="2 1" src="https://github.com/user-attachments/assets/119b27d1-eed9-480b-8166-4defacaffe24" />

---

### **Step 3: Host Key Verification (Security First! 🔐)**

When I connected for the first time, PuTTY threw up a warning—and that's a **good thing**. This is SSH's way of making sure I'm connecting to the right server and not some imposter.

**Here's what popped up:**
- **Server IP:** 16.144.132.13:22
- **Key Type:** ssh-ed25519

I had three options:
1. **Accept and cache** the key (recommended for servers you trust)
2. **Connect once** without caching (for one-off connections)
3. **Cancel** if something looks fishy

Since I knew this was my EC2 instance, I went ahead and accepted the key.

<img width="302" height="272" alt="2 2" src="https://github.com/user-attachments/assets/dcc3b584-a5dd-4e5a-814a-7fe9e04e6382" />

---

### **Step 4: Logging Into Amazon Linux 2**

After accepting the host key, boom—I was in! 

**Login Details:**
- **Username:** ec2-user (default for Amazon Linux)
- **Authentication:** Public key (imported-openssh-key)
- **OS:** Amazon Linux 2 (AL2)

The system greeted me with a friendly reminder that Amazon Linux 2 reaches end-of-life on **June 30, 2026**. It also suggested upgrading to Amazon Linux 2023, which is supported until **March 15, 2028**—something to keep in mind for future projects!

<img width="317" height="285" alt="2 3" src="https://github.com/user-attachments/assets/151336cf-b7fa-4ee0-be8e-54aec2915cbd" />

---

### **Step 5: Exploring the `man` Command**

Once I was logged in, I wanted to demonstrate how to use Linux's built-in documentation system. The **man** (manual) command is your best friend when you need to learn about any command.

I ran:
bash
man man

This showed me the manual page for the **man** command itself—very meta! The manual pages are organized into sections and give you everything you need to know about a command: syntax, options, examples, and more.

**Key Sections I Found:**
- **NAME:** What the command does
- **SYNOPSIS:** How to use it
- **DESCRIPTION:** Detailed explanation

> 💡 **Quick tip:** Use man <command> anytime you're stuck. For example, man ls shows you everything about the ls command.

<img width="497" height="287" alt="4" src="https://github.com/user-attachments/assets/919528b8-201d-4bea-a83a-e8c5cc828569" />
<br></br>
<img width="851" height="484" alt="5" src="https://github.com/user-attachments/assets/3f14b5c3-7c9c-419a-be0c-aeb21e6a3d27" />

---

### **Step 6: Lab Complete! 🎉**

And that's a wrap! I successfully completed the lab. Here's what I accomplished:

✅ Configured PuTTY for SSH  
✅ Optimized connection settings  
✅ Verified the host key  
✅ Logged into an EC2 instance  
✅ Used the **man** command to explore documentation  

**Next Steps:**
- Save your PuTTY session for quick access later
- Document any custom commands or configurations you use
- Consider upgrading to Amazon Linux 2023 for extended support

<img width="639" height="59" alt="6" src="https://github.com/user-attachments/assets/26c6211d-4562-47a2-8390-cd550d13fd11" />

---

## 📊 Quick Reference Guide

Here's a handy table summarizing each step:

| Step | What I Did | Key Detail | Result |
|------|-----------|-----------|---------|
| **1** | Configured PuTTY session | Host: `16.144.132.13`, Port: `22` | Ready to connect |
| **2** | Optimized connection | Keepalives: `30s`, TCP_NODELAY enabled | Smoother session |
| **3** | Verified host key | Type: `ed25519`, SHA256 fingerprint | Security confirmed |
| **4** | Logged into EC2 | User: `ec2-user`, Auth: Public key | Access granted  |
| **5** | Explored `man` command | Ran `man man` | Learned about docs |
| **6** | Finished lab | All tasks completed | Success!  |

---

## 🛠️ Tools & Technologies Used

- **PuTTY** - SSH client for Windows
- **Amazon EC2** - Cloud compute instance
- **Amazon Linux 2** - Operating system
- **SSH (ed25519)** - Secure shell protocol

---

## 📝 Final Thoughts

This lab was a great refresher on SSH basics and PuTTY configuration. The key takeaways for me were:

1. **Always verify host keys** on first connection
2. **Connection optimization** can make a real difference in user experience
3. **Manual pages** are incredibly useful—get comfortable with **man**!

---
