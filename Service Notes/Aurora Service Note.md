# Service Note: AWS Aurora PostgreSQL Database Deployment

**Date:** 28-11-2025  
**Created by** Brite Sendedza
**Project:** Aurora PostgreSQL Database Setup  
**Region:** US West (Oregon)  
**Status:** Completed Successfully

---

## Overview

Alright, so I finally got my AWS Aurora PostgreSQL database cluster deployed and running. This service note is basically me documenting everything I did, the AWS tools I worked with, and honestly some stuff I struggled with (especially around primary keys). But hey, I figured it out and that's what matters.

---

## What I Built

**Database Cluster:** Aurora (PostgreSQL Compatible) - Version 11.9  
**Instance Type:** db.t3.medium  
**Template:** Dev/Test  
**Initial Database:** world  
**Estimated Monthly Cost:** $61.86 USD

---

## AWS Tools & Services I Used

### 1. **Amazon Aurora (PostgreSQL Compatible)**
So Aurora is basically AWS's souped-up version of PostgreSQL. It's fully managed, which is great because I don't have to worry about backups, patches, or scaling - AWS handles all that automatically. What's really cool is that Aurora separates the compute layer from storage, so I can scale them independently if I need to. I went with the PostgreSQL-compatible version because I needed those specific PostgreSQL features, but I wanted the performance boost that Aurora provides.

### 2. **Amazon RDS Console**
This is where I spent most of my time managing the database. The RDS console is like mission control for all your database stuff - creating clusters, tweaking security settings, watching performance metrics, and grabbing those connection endpoints. Once you figure out where everything lives, it's actually pretty straightforward to use.

### 3. **Amazon VPC (Virtual Private Cloud)**
I used my **LabVPC** to keep the database isolated in its own private network. Think of a VPC as your own private corner of AWS's massive cloud infrastructure - nothing gets in or out unless you say so. I also had to set up this **dbsubnetgroup** thing, which basically tells AWS exactly which subnets my database is allowed to exist in. Network stuff can get confusing, but it's worth getting right.

### 4. **Security Groups**
These are like firewall rules for your AWS stuff. I set up **DBSecurityGroup** to control who could actually talk to my database. I locked it down pretty tight - only resources inside my VPC can connect. No public internet access whatsoever. This might seem paranoid, but honestly, you don't want your database exposed to the internet. Trust me on this one.

### 5. **Amazon EC2 (Elastic Compute Cloud)**
I needed an EC2 instance (I called it **Command Host**) to actually connect to the database. Since I deliberately made my database not publicly accessible, I had to have something inside the same VPC that could reach it. EC2 is just AWS's way of saying "virtual server." Pretty simple concept.

### 6. **AWS Systems Manager - Session Manager**
Okay, this tool is awesome. Instead of dealing with SSH keys and all that headache, Session Manager lets me connect to my EC2 instance right through my browser. Way easier, and actually more secure too since I don't have to open SSH ports or manage a bunch of keys. Wish I'd known about this sooner, honestly.

---

## Key Configuration Details

### Network & Security
- **VPC:** LabVPC
- **Subnet Group:** dbsubnetgroup
- **Security Group:** DBSecurityGroup
- **Public Access:** Disabled (VPC-only access)
- **Port:** 5432 (standard PostgreSQL)

### Database Credentials
- **Master Username:** admin
- **Password Management:** Self-managed
- **Initial Database Name:** world

### High Availability
- **Multi-AZ Deployment:** Disabled (single instance for dev/test)
- **Deletion Protection:** Disabled

---

## What I Actually Did

### Phase 1: Database Provisioning
So first, I had to actually get this thing set up. I went through the RDS console and configured my Aurora cluster. I picked PostgreSQL 11.9 as the engine version - nothing too cutting edge, but solid and stable. For the instance size, I went with db.t3.medium because it's a good sweet spot for testing without burning through my budget.

Then came the network stuff. I placed everything in my LabVPC with the proper subnet groups to keep it isolated. Security-wise, I locked it down with DBSecurityGroup so only things inside my VPC could reach it. After confirming all my settings, I hit create and just waited while AWS did its thing. Took a few minutes, but eventually everything was up and running.

### Phase 2: Client Setup & Connection
Once the database was ready, I needed to actually connect to it. I checked that my Command Host EC2 instance was running (it was), then connected to it using Session Manager through my browser - so much easier than SSH.

First thing I did once I was logged in was install the database client tools with `sudo yum install mariadb -y`. Yeah, I know it says MariaDB, but it works perfectly fine for connecting to PostgreSQL. Then I grabbed the writer endpoint from the RDS console and successfully connected on port 5432. Felt good to see that connection establish!

### Phase 3: Database Operations
Now for the fun part - actually using the database! I created my **country** table with all the columns I needed. Made sure to define a PRIMARY KEY on the Code column (more on that in the challenges section - that gave me some headaches).

After that, I loaded some test data into the table and ran a query to filter countries by GNP and population. The query worked perfectly and gave me back Australia and Thailand, which is exactly what I was expecting. Everything was working smoothly at this point.

---

## Challenges

### Understanding Primary Keys and How They Work in Their Depth

Okay, so this is where I hit a bit of a wall. I mean, I knew primary keys were important - everyone says they are - but I realized I didn't really understand them as deeply as I should. I needed to really get this right because primary keys are fundamental to good database design.

After doing some digging and reading through documentation, I learned there are actually **two types of primary keys**:

1. **Simple Primary Key (Single Column)**
   - This is just one column that uniquely identifies each row
   - In my country table, I used Code as the primary key
   - Each country code like "AUS" for Australia is completely unique, so it works perfect

2. **Composite Primary Key (Multiple Columns)**
   - This uses two or more columns together to make a unique identifier
   - Like if I had an order_items table, I might use order_id + product_id combined
   - Neither column by itself is unique, but together they are

The part that really confused me was figuring out when to use which type. Also, I learned that primary keys automatically create indexes behind the scenes, which speeds up queries but takes up storage space. There's always a tradeoff, right?

---

## Solutions

### How I Actually Figured This Out

**1. Went Back to Basics**
First thing I did was step back and review what actually makes a good primary key. It has to be unique (obviously), it can never be null, and ideally it should never change. For my country table, the country code was perfect because every country has a unique ISO code that doesn't change. Simple as that.

**2. Sketched Out Different Scenarios**
I actually grabbed a piece of paper and started drawing out different table relationships. For the country table, a simple primary key made total sense since country codes are naturally unique. But then I started thinking about other scenarios where I'd need composite keys - like those junction tables you use for many-to-many relationships. Writing it out really helped it click.

**3. Tested Everything**
After I created my table with Code as the PRIMARY KEY, I intentionally tried to break it. I attempted to insert duplicate country codes to see what would happen. The database rejected them, which meant the constraint was actually working. I also checked to make sure the index was automatically created and being used in my queries. Yep, it was there doing its job.

**4. Wrote Down My Own Best Practices**
I started keeping notes for myself about when to use what:
- Simple primary keys: Use these when you've got a natural unique identifier (like IDs, codes, or usernames)
- Composite primary keys: Use these for junction tables or when you need multiple columns to guarantee uniqueness
- If there's no natural key that makes sense, just add a surrogate key (like an auto-incrementing ID)
- Remember that primary keys create indexes automatically, which is great for performance

**5. Applied It in Real Life**
I went back and checked my actual queries to make sure they were using the primary key index properly. I looked at the query plans and verified everything was optimized. I also tried inserting some bad data on purpose - duplicates, nulls, all that stuff - to confirm my constraints were really working. Everything behaved exactly like it should, which told me I'd set it up correctly.

Honestly, this hands-on trial-and-error approach is what really made it stick. Now when I'm designing tables, I actually feel confident about choosing the right primary key strategy instead of just guessing.

---

## Verification & Testing

**Connection Test:** Connected from Command Host to Aurora cluster without any issues  
**Table Creation:** Created country table with PRIMARY KEY working correctly  
**Data Operations:** Inserted and queried data - everything smooth  
**Query Results:** Got back Australia and Thailand like I expected  
**Security:** Confirmed no public access is possible, VPC-only connectivity working perfectly

---

## Stuff I Learned

1. Aurora PostgreSQL gives you the power of PostgreSQL but with less babysitting - AWS manages most of the annoying stuff
2. Seriously, never enable public access on production databases. Use VPC isolation. Just don't do it.
3. Security groups are super important - they're your first line of defense, so configure them carefully
4. Session Manager is a game changer for EC2 access. Way easier than traditional SSH
5. You really need to understand primary keys deeply - knowing the difference between simple and composite keys matters
6. Testing your constraints by intentionally breaking them is actually a smart way to verify they work
7. The db.t3.medium instance is a solid choice for dev/test stuff without costing a fortune

---

## Quick Reference

- **Database Endpoint (Writer):** You can find this in RDS Console under Connectivity & Security
- **Port:** 5432
- **Security Group:** DBSecurityGroup (managed in the VPC console)
- **Command Host:** My EC2 instance that lives inside the VPC for database access
- **Monthly Cost:** About $62 - not too bad for what I'm getting

---
**Final Thoughts:** This whole setup was for dev/test purposes, so I kept things simple. If I was doing this for real production use, I'd definitely enable Multi-AZ deployment, turn on deletion protection, set up proper automated backups, and add way more monitoring. The fact that this database is completely cut off from the public internet is exactly what I wanted - security first, always.

**Date:** 28-11-2025  
**Created by** Brite Sendedza
