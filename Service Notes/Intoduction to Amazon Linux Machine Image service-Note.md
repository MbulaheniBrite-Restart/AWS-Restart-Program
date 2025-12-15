# 📋 Service Note: Remote SSH Session to Amazon Linux 2 EC2 Instance

---

## 📌 Overview

**Date:** 08-Dec-2025   
**Service Type:** Remote System Access & Configuration  
**Technician:** Brite Sendedza
**Status:** ✅ Completed Successfully

---

## 🎯 Service Summary

I successfully established a secure SSH connection to an Amazon Linux 2 EC2 instance using PuTTY. The session was optimized for performance and stability, with proper security verification performed.

---

## 🔧 Services Performed

### **1. SSH Connection Setup**

Configured PuTTY for secure remote access:
- **Remote Host:** `16.144.132.13`
- **Protocol:** SSH (Port 22)
- **Terminal Settings:** Close window only on clean exit

**What is PuTTY?**  
PuTTY is a free SSH client for Windows that lets you securely connect to remote Linux servers. It's the standard tool for managing cloud servers from Windows machines.

---

### **2. Connection Optimization**

Applied performance tuning settings:
- **Keepalive Interval:** 30 seconds (prevents session timeout)
- **TCP_NODELAY:** Enabled (reduces latency)
- **IP Version:** Auto-detect

---

### **3. Host Key Verification**

Verified server identity on first connection:
- **Host:** `16.144.132.13:22`
- **Key Type:** `ssh-ed25519`
- **Fingerprint:** `SHA256:gcdeKJwEV3Hg1jLg0huRHuuMdE8gWXvAQPlQlBhXYfo`
- **Action:** Accepted and cached

**What is Host Key Verification?**  
This is SSH's built-in protection against man-in-the-middle attacks. The server's fingerprint acts like a digital ID card—if it changes unexpectedly, you'll know something's wrong.

---

### **4. EC2 Instance Authentication**

Successfully authenticated to the instance:
- **Username:** ec2-user
- **Authentication:** Public key (imported-openssh-key)
- **Operating System:** Amazon Linux 2

**What is Amazon EC2?**  
Amazon Elastic Compute Cloud (EC2) is AWS's virtual server service. You rent computing power in the cloud instead of buying physical hardware—scalable, flexible, and cost-effective.

**What is Amazon Linux 2?**  
Amazon Linux 2 is AWS's Linux distribution optimized for EC2. It comes pre-configured with AWS tools and receives regular security updates.

**System Notice:**
- **AL2 End of Life:** June 30, 2026
- **Recommended Upgrade:** Amazon Linux 2023 (supported until March 15, 2028)

---

### **5. System Documentation Access**

Verified access to Linux manual pages:
bash
man man


Confirmed system documentation is properly installed and accessible.

---

## 🎓 Personal Experience

- **Host key verification is your friend** - It's protecting against attacks, not just an annoying popup. I now always verify fingerprints properly.
- **Connection timeouts used to frustrate me** - Setting the 30-second keepalive interval solved sessions dying when I stepped away.
- **TCP_NODELAY makes a real difference** - Noticeably reduced lag when typing commands over the network.
- **Save your PuTTY sessions!** - I wasted time re-entering details until I started saving session profiles for different environments.
- **The `man` command is underrated** - More efficient than Googling and sorting through random blog posts.
- **Amazon Linux 2's EOL date** - Need to start planning upgrades now rather than scrambling in 2026.

---

## 📝 Add For Me

**Things to remember:**
- Save session configuration immediately after testing
- Keep a secure note of host key fingerprints for verification
- Document any custom configurations during the session
- Check system uptime and pending updates before starting work

**Simple but important:**
- Double-check environment (dev/staging/prod) before running commands
- Close sessions properly with `exit` command rather than closing the window
- Wait 30 seconds before reconnecting after unexpected disconnection
- Bookmark the AWS EC2 console page for quick instance status checks

---

## Verification

- SSH connection established successfully
- Authentication working (public key)
- Connection stable (no timeouts)
- Terminal responsiveness optimal
- System documentation accessible

---

## 📊 Connection Summary

| Parameter | Value | Status |
|-----------|-------|--------|
| Host IP | `16.144.132.13` | Reachable |
| Port | `22` | Open |
| Authentication | Public Key | Successful |
| User Account | `ec2-user` | Active |

---

## ✍️ Sign-Off

**Service Completed By:** Brite Sendedza  
**Date:** 08-Dec-2025  
**Status:** All objectives achieved. Instance accessible and properly configured.

---
