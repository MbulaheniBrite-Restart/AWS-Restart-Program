## My VPC and Web Server Deployment - Service Note

## 📋 Quick Overview

**Lab Date:** 17-11-2025  
**Lab Type:** VPC Creation and EC2 Web Server Deployment  
**Focus Area:** AWS Networking Fundamentals  
**Customer Scenario:** Fortune 100 Enterprise Network Architecture  
**Completed By:** Brite Sendedza  
**Status:** Successfully Completed

---

## 🎯 What I Set Out to Do

I wanted to build a complete, customized network environment from scratch using Amazon VPC and deploy a working web server into it. This lab simulated building real network architecture for an enterprise customer—the kind of stuff you'd actually do in production.

**My Specific Goals:**
* Create my own Virtual Private Cloud (VPC)
* Set up public and private subnets within the VPC
* Configure security groups (firewall rules)
* Launch an EC2 instance and make it accessible from the internet

---

## The AWS Services I Used

### Amazon VPC (Virtual Private Cloud)
**What It Is:** VPC is your own isolated network space in AWS. Think of it as your private data center in the cloud where you have complete control over IP addressing, subnets, routing, and security.

**How I Used It:** I created a VPC called Lab VPC with a CIDR block of 10.0.0.0/16, giving me over 65,000 IP addresses to work with. This became the foundation for everything else.

### Subnets
**What They Are:** Subnets divide your VPC into smaller network segments. You can have public subnets (accessible from the internet) and private subnets (isolated from direct internet access).

**How I Used Them:** I created both public and private subnets across different Availability Zones. My web server went into a public subnet so it could be reached from the internet, while backend resources would go in private subnets.

### Internet Gateway (IGW)
**What It Is:** The Internet Gateway is the bridge between your VPC and the internet. Without it, nothing in your VPC can communicate with the outside world.

**How I Used It:** The automated VPC creation attached an IGW to my VPC, enabling internet connectivity for resources in public subnets.

### Route Tables
**What They Are:** Route tables contain rules that determine where network traffic gets directed. They're like the GPS for your network.

**How I Used Them:** I configured route tables to direct internet-bound traffic (0.0.0.0/0) to the Internet Gateway for public subnets, while keeping private subnet traffic isolated.

### Security Groups
**What They Are:** Security Groups are virtual firewalls that control inbound and outbound traffic at the instance level. They use allow rules only.

**How I Used Them:** I created a security group called `Web` that allows HTTP traffic (Port 80) so people can access my web server from their browsers.

### Amazon EC2
**What It Is:** EC2 provides resizable virtual servers in the cloud. You can configure them however you need.

**How I Used It:** I launched a t3.micro instance running a web server in my public subnet, giving it both public and private IP addresses.

---

## How I Built Everything

### Step 1: Creating My VPC Foundation

**Getting Started:**
I opened the VPC dashboard and selected "VPC and more" to use the automated workflow. This was a huge time-saver because it creates a standard two-tier architecture automatically.

**Configuring My VPC:**
* **Name:** Lab VPC
* **CIDR Block:** 10.0.0.0/16 (plenty of IP addresses!)
* **Result:** The wizard created my VPC, public and private subnets, route tables, and an Internet Gateway all at once

**Verification:**
I checked the VPC dashboard and confirmed that:
* My VPC (vpc-0431518c...) was created
* DNS hostnames and resolution were enabled
* All subnets, route tables, and the IGW were properly attached

I could see four subnets were automatically created by the workflow.

---

### Step 2: Customizing My Network

**Adding Another Public Subnet:**
I wanted an additional public subnet, so I created Public Subnet 2 within my Lab VPC. This gives me more flexibility for distributing resources.

**Verifying the Subnet:**
The dashboard confirmed my new subnet (subnet-0ada8314...) was successfully created and available.

**Associating with Route Tables:**
This was important—I had to make sure each subnet was associated with the correct route table:
* **Public Subnets:** Associated with the Public Route Table (rtb-0009f...), which routes internet traffic to the IGW
* **Private Subnets:** Associated with the Private Route Table (rtb-034fa...), which keeps backend resources isolated

---

### Step 3: Setting Up Security and Launching My Web Server

**Creating the Security Group:**
I went to the Security Groups section and created a new group called Web (sg-0f24364f...).

**Configuration:**
* **Description:** "Enable HTTP access"
* **Inbound Rules:** 1 rule allowing HTTP traffic (Port 80)
* **Outbound Rules:** 1 default rule allowing all outbound traffic

**Launching My EC2 Instance:**
I launched an EC2 instance called Web Server 1 using a t3.micro instance type (perfect for testing) and placed it in my public subnet.

**Instance Details:**
* **State:** Running
* **Status Checks:** 3/3 passed
* **Public IPv4:** 35.91.193.115
* **Private IPv4:** 10.0.2.9

---

### Step 4: Testing and Validation

**Verifying Connectivity:**
This was the moment of truth! I:
1. Waited for the instance to show 2/2 checks passed
2. Copied the Public IPv4 DNS value
3. Pasted it into my web browser and hit Enter

**Result:** Success! My web server was accessible from the internet. The page loaded perfectly, confirming that my entire network configuration—VPC, subnets, route tables, IGW, security groups, and EC2 instance—was working together flawlessly.

---

## Challenge I Faced

### Seeing Myself Actually Excel at This

**What Happened:** My main challenge wasn't technical—it was personal. I've always had this hunger to learn how to properly create VPCs and all their resources, but I wasn't sure if I could actually do it well. Watching myself successfully configure everything—the VPC, subnets, route tables, security groups, and getting that web server running—was honestly a bit surreal.

**Why This Mattered:** For so long, VPC networking felt like this intimidating, complex topic that "real" cloud engineers did. Actually doing it myself and seeing it work was a confidence breakthrough.

---

## My Experience and Key Takeaways

**The Experience:**
* **Empowerment:** There's something incredible about creating an entire network from scratch and watching it come to life. When that web page loaded in my browser, I realized I had built the entire path that request took—from the internet, through the IGW, into my VPC, past my security group, to my EC2 instance.

* **Understanding Through Doing:** Reading about VPCs is one thing; actually creating subnets, configuring route tables, and associating them correctly gave me a deep, practical understanding I never had before.

* **The "Aha!" Moment:** It all clicked when I saw how each piece connects. The VPC is the container, subnets segment the space, route tables direct traffic, the IGW provides internet access, security groups control what gets through, and EC2 instances run your applications. It's like building with LEGO blocks—each piece has a purpose.

**Key Takeaways:**
* **VPC Automation is Powerful:** Using "VPC and more" saved me tons of time by automating the creation of common network components. But understanding what it creates is crucial.

* **Route Tables are Everything:** If your route tables aren't configured correctly, traffic goes nowhere. The route to 0.0.0.0/0 → IGW is what makes a subnet "public."

* **Security Groups are Your Friend:** They're stateful (return traffic is automatically allowed) and work at the instance level. Understanding this made security configuration so much easier.

* **Public vs. Private Matters:** Public subnets need routes to the IGW and resources need public IPs. Private subnets don't route to the IGW, keeping resources isolated. This architectural pattern is fundamental to AWS.

* **I Can Actually Do This:** The biggest takeaway? I have the skills to design and build real network architectures. That hunger to learn wasn't just wishful thinking—I proved to myself that I can excel at this.

---

## What I Accomplished

| Component | What I Built | Status |
| :--- | :--- | :--- |
| VPC | Created Lab VPC with 10.0.0.0/16 CIDR | Complete |
| Subnets | Public and private subnets across AZs | Complete |
| Route Tables | Configured public and private routing | Complete |
| Internet Gateway | Attached IGW for internet connectivity | Complete |
| Security Group | Created Web SG with HTTP access | Complete |
| EC2 Instance | Launched Web Server 1 (t3.micro) | Complete |
| Connectivity Test | Successfully accessed web server via browser | Complete |

**Key Achievement:** Built a complete, production-ready network architecture from scratch and deployed a functioning web server accessible from the internet.

---

## 🎓 What This Taught Me

### Confidence in Cloud Architecture
I came into this lab eager but uncertain. I left it knowing I can design and implement real AWS network architectures. That's huge for my career growth.

### The Importance of Fundamentals
Understanding how VPCs, subnets, routing, and security work together is foundational for everything else in AWS. You can't build complex architectures without these basics.

### Hands-On Learning Works
No amount of reading could have given me the confidence that actually doing this work provided. Getting my hands dirty and troubleshooting along the way cemented the knowledge.

### Enterprise-Ready Skills
This lab simulated building network infrastructure for a Fortune 100 customer. I now have practical experience with the same patterns and architectures used in real enterprise environments.

---

## What I'd Do Next

* **Multi-Tier Architecture:** Build a full three-tier architecture with web, application, and database layers
* **NAT Gateway Implementation:** Set up NAT Gateways so private subnet resources can access the internet securely
* **VPC Peering:** Connect multiple VPCs together for more complex network topologies
* **Network ACLs:** Add another layer of security at the subnet level
* **VPC Flow Logs:** Implement logging to monitor and troubleshoot network traffic

---

## ✍️ Final Thoughts

**Lab Status:** Successfully completed  
**Confidence Level:** High—I can now build VPC architectures independently  
**Personal Growth:** Proved to myself I can excel at cloud networking  
**Would I Recommend This?** Absolutely! Essential for anyone serious about AWS

**My Closing Notes:** This lab was transformational for me. I didn't just learn how to create a VPC—I proved to myself that I can design and implement real network architectures. That hunger to learn VPC networking has been satisfied, but it's been replaced with excitement to tackle even more complex networking challenges. Seeing my web server accessible from the internet, knowing I built every piece of the network it runs on, was genuinely empowering. I'm ready to apply these skills to real-world projects and continue growing as a cloud engineer.

**Completed On:** 17-11-2025  
**By:** Brite Sendedza
