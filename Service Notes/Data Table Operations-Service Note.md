# Service Note: Database Administration

**Date:** 01-12-2025  
**Created by*** Brite Sendedza
**Project:** MariaDB Server Administration on EC2  
**Region:** US West (Oregon) - us-west-2  
**Status:** Completed Successfully

---

## Overview

So I needed to access an EC2 instance and perform some basic database administration tasks on a MariaDB server. This service note covers how I connected securely to the instance, set up a database from scratch, and ran through some fundamental database operations.

---

## What I Accomplished

**EC2 Instance:** Command Host (t3.micro)  
**Database Server:** MariaDB 10.5.29  
**Database Created:** lab_db  
**Table Created:** inventory (with auto-incrementing primary key)  
**Test Data:** Successfully inserted and queried

---

## AWS Tools & Services I Used

### 1. **Amazon EC2 (Elastic Compute Cloud)**
EC2 is basically AWS's virtual server service. My **Command Host** instance was a t3.micro - small and efficient, perfect for this kind of work. The instance was already running and healthy when I started, which made things easy. EC2 gives you complete control over your virtual machines, and you only pay for what you use.

### 2. **AWS Systems Manager - Session Manager**
Okay, this tool is seriously underrated. Session Manager lets you connect to your EC2 instances directly through your browser without dealing with SSH keys, bastion hosts, or opening up port 22 to the internet. It's way more secure because all the authentication happens through AWS IAM, and you get full session logging for compliance. Once I started using Session Manager, I honestly never wanted to go back to traditional SSH.

### 3. **MariaDB Database Server**
MariaDB is an open-source relational database - think of it as a community-driven fork of MySQL. It was already installed on my EC2 instance, running version 10.5.29. It's fully compatible with MySQL, super reliable, and handles all the standard SQL operations you'd expect. Perfect for dev work, testing, and even production if you need a solid relational database.

---

## Instance Configuration

**Instance Name:** Command Host  
**Instance Type:** t3.micro  
**Region:** us-west-2 (Oregon)  
**Status:** Running  
**Health Checks:** 3/3 passed  
**Connection Method:** AWS Systems Manager - Session Manager  
**Database Server:** MariaDB 10.5.29  
**Root Password:** Managed (rest@rt!)

---

## What I Actually Did

### Phase 1: Connecting to the EC2 Instance
First, I made absolutely sure I was in the right AWS region - us-west-2 (Oregon). Can't stress enough how important this is. Then I navigated to the EC2 Dashboard and found my **Command Host** instance in the list. It was running smoothly with all health checks passing.

I clicked on the instance and selected **Session Manager** as my connection method. Within seconds, I had a browser-based terminal session open. No SSH keys, no hassle, just instant access. Pretty slick.

### Phase 2: Accessing MariaDB
Once I was on the instance, I elevated my privileges with sudo su to get root access. Then I launched the MariaDB client using the root credentials:

bash
mysql -u root --password='rest@rt!'


The connection established immediately, and I could see I was running MariaDB version 10.5.29. Ready to go.

### Phase 3: Database Setup
I started by checking what databases already existed with SHOW DATABASES; - just the default system databases, as expected.

Then I created my own database called **lab_db** and switched to it using the USE command. Now all my subsequent commands would run in the context of this database.

### Phase 4: Table Creation and Data Operations
Time to create a table! I designed a simple **inventory** table with three columns:
- id - Auto-incrementing integer, set as the primary key
- item_name - VARCHAR field to store the item name
- quantity - Integer field for tracking quantities

After creating the table, I inserted a test record - a laptop with a quantity of 50. Then I ran a SELECT query to verify everything worked correctly. Sure enough, there was my record with an auto-generated ID of 1. Everything was functioning perfectly.

Finally, I exited the MariaDB session with the EXIT; command and closed my Session Manager connection.

---

## Challenges

### Working with Command-Line Database Administration
- **Initial intimidation:** Coming from GUI database tools, the command-line interface felt pretty bare-bones at first
- **Syntax precision:** SQL syntax errors are unforgiving - one misplaced comma or missing semicolon and the command fails
- **No visual confirmation:** Without a GUI, I had to rely entirely on command outputs to verify my actions were successful
- **Session management:** Keeping track of which database context I was in (especially after using USE command) required focus

---

## Solutions

### How I Made It Work (And Why It Was Worth It)

**1. Started Simple and Built Confidence**
I didn't try to do anything fancy right away. I started with basic commands like SHOW DATABASES; just to get comfortable with the interface. Seeing successful outputs, even for simple commands, helped me build confidence. By the time I was creating tables and inserting data, the command line didn't feel intimidating anymore.

**2. Developed a Command Verification Habit**
After every action, I got into the habit of verifying it immediately. Created a database? Run SHOW DATABASES; to see it in the list. Inserted data? Run SELECT * to confirm it's there. This constant verification loop made me feel way more in control and caught any issues right away.

**3. Embraced the Learning Curve**
Yeah, I made syntax errors. Yeah, I forgot semicolons a few times. But honestly? Each mistake taught me something. The error messages, while sometimes cryptic, actually forced me to understand what I was doing rather than just clicking buttons in a GUI. Looking back, this made me a better database administrator.

**4. Discovered the Power of Session Manager**
Once I got comfortable with Session Manager, I realized how much easier it made everything. No SSH key management, no security group rules for port 22, no worrying about my IP changing. Just click connect and start working. This tool alone made the whole experience smoother than I expected.

**5. Appreciated the Direct Control**
Here's the thing - once you get past the initial learning curve, the command line is actually faster and more powerful than any GUI. I could execute commands instantly, chain operations together, and I had complete visibility into exactly what was happening. No abstraction layers, no hidden processes. Just direct control over the database.

**The Real Takeaway:**
The experience was absolutely worth it. Sure, it felt uncomfortable at first, but pushing through that discomfort gave me skills I'll use forever. I can now confidently work with databases in any environment - EC2 instances, on-premises servers, containers, wherever. The command line isn't scary once you spend time with it; it's actually incredibly empowering. Would I go back to only using GUIs? Not a chance.

---

## Verification & Testing

**EC2 Connection:** Successfully connected via Session Manager  
**MariaDB Access:** Logged in as root user without issues  
**Database Creation:** Created lab_db successfully  
**Table Structure:** Created inventory table with proper schema and primary key  
**Data Insertion:** Inserted test record successfully  
**Data Retrieval:** SELECT query returned expected results  
**Session Cleanup:** Exited cleanly and terminated connection

---

## Key Takeaways

1. Session Manager is hands-down the best way to connect to EC2 instances - more secure and way easier than SSH
2. Command-line database administration feels intimidating at first but becomes second nature with practice
3. Always verify your actions immediately with a follow-up query - it catches mistakes fast
4. MariaDB is solid, stable, and fully MySQL-compatible - great choice for relational databases
5. The t3.micro instance type is perfect for dev/test database work without burning budget
6. Taking the time to learn command-line SQL makes you a more versatile and capable administrator
7. Every syntax error is a learning opportunity, not a failure

---

## Commands Reference

### Connection & Access
bash
# Elevate privileges
sudo su

# Connect to MariaDB
mysql -u root --password='rest@rt!'


### Database Operations
sql
-- View existing databases
SHOW DATABASES;

-- Create new database
CREATE DATABASE lab_db;

-- Switch to database
USE lab_db;

-- Create table with primary key
CREATE TABLE inventory (
  id INT NOT NULL AUTO_INCREMENT,
  item_name VARCHAR(100) NOT NULL,
  quantity INT NOT NULL,
  PRIMARY KEY (id)
);

-- Insert data
INSERT INTO inventory (item_name, quantity) 
VALUES ('Laptop', 50);

-- Query data
SELECT * FROM inventory;

-- Exit MariaDB
EXIT;


---

## Next Steps

Now that I've got the basics down, here's what I'm thinking for next steps:
- Create more complex table structures with foreign key relationships
- Practice JOIN operations across multiple tables
- Set up automated backups for the database
- Explore user management and permission controls in MariaDB
- Try some performance optimization with indexes
- Maybe containerize this setup with Docker for easier deployment

---

## Quick Reference

**Instance:** Command Host (t3.micro)  
**Region:** us-west-2 (Oregon)  
**Connection:** Session Manager (browser-based)  
**Database Server:** MariaDB 10.5.29  
**Database Name:** lab_db  
**Table:** inventory (id, item_name, quantity)

---

**Final Thoughts:** This whole exercise really drove home how valuable it is to be comfortable with command-line database work. Sure, GUIs are nice and have their place, but knowing how to work directly with SQL commands gives you so much more flexibility. Whether you're troubleshooting production issues, working on remote servers, or automating tasks with scripts, these command-line skills are essential. Glad I took the time to work through this properly instead of taking shortcuts.
