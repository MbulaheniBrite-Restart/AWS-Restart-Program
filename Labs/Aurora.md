# How I Deployed My AWS Aurora (PostgreSQL) Database

## What This Project Is About

So basically, I needed to get an **AWS Aurora (PostgreSQL Compatible)** database up and running. This document is me walking you through exactly how I did it - from the initial setup, getting the network security sorted, actually spinning up the database, connecting to it from an EC2 instance, and then running some SQL queries to work with my data.

---

## Part I. Setting Up and Configuring My Database

### 1. Getting to the Console
First thing I did was jump into the AWS Console and navigate to the **Aurora and RDS** service. I double-checked that I was in the right region because, trust me, you don't want to be creating resources in the wrong place.

Region: **US West (Oregon)**

![A](https://github.com/user-attachments/assets/98adec21-fc87-4274-887c-5e5fba1e8acb)

### 2. Starting the Database Creation
Once I was in the right spot, I opened up the **Databases** dashboard - it was empty at this point - and clicked **Create database** to get the ball rolling.

![B](https://github.com/user-attachments/assets/460ce969-6749-4a13-acb1-d8b062f5c29e)

### 3. Choosing My Database Engine
Now here's where I had to make some choices. I went with the standard configuration method because I wanted full control over everything. For the database technology, I picked **Aurora (PostgreSQL Compatible)** since that's what I needed for this project.

Engine Type: **Aurora (PostgreSQL Compatible)**

![C](https://github.com/user-attachments/assets/b5d71697-2b26-4967-822b-a32d8fc8b139)

### 4. Picking the Template and Version
Next up, I had to tell AWS what I'd be using this database for. I selected **Dev/Test** since this wasn't going into production just yet. I also made sure to grab the right PostgreSQL version I needed.

Template: **Dev/Test**  
Version: **PostgreSQL 11.9**

![D](https://github.com/user-attachments/assets/b7ce04de-277d-4bf6-a22c-8d93b53e2970)

### 5. Setting Up My Login Credentials
Time to set up how I'd actually log into this thing. I configured the master credentials - basically the admin account that has full access to the database. I kept it simple with **admin** as the username and chose to manage the password myself rather than letting AWS handle it.

Master Username: **admin**  
Password Management: **Self managed**

![E](https://github.com/user-attachments/assets/088612c5-3484-4189-a9c0-14cdab76418c)

### 6. Configuring the Instance Size
Here's where I decided on the compute power. I went with a **db.t3.medium** instance - nothing too crazy, but enough for what I needed. I also had to choose whether I wanted high availability with multiple replicas across different zones, but I decided to skip that for now to keep things simple.

Class: **db.t3.medium**  
Multi-AZ: **Don't create an Aurora Replica**

![F](https://github.com/user-attachments/assets/900b5063-98fe-41cd-bccd-47fe827a0927)

### 7. Placing It in My Network
Now I needed to figure out where in my network this database would live. I assigned it to my **LabVPC** and used the **dbsubnetgroup** I'd already set up. This is important because it keeps the database properly isolated in my network architecture.

VPC: **LabVPC**  
Subnet Group: **dbsubnetgroup**

![G](https://github.com/user-attachments/assets/05d66d14-7aaa-4cac-b31c-bb73fcbaf610)

### 8. Setting Up Security (My Firewall Rules)
Security time! I really wanted to lock this down, so I made sure to configure it so only resources inside my VPC could reach it. No public internet access whatsoever - that's just asking for trouble. I attached it to my **DBSecurityGroup** which has all the right rules already configured.

Public Access: **No**  
Security Group: **DBSecurityGroup**

![H](https://github.com/user-attachments/assets/a82d3b22-b372-436a-b1ab-152f46d127be)

### 9. Naming My Database
I needed to give my initial database schema a name, so I called it **world**. This is the database that gets created automatically when the cluster spins up.

Initial DB Name: **world**

![I](https://github.com/user-attachments/assets/425b6c7f-f9fd-443a-8a0d-64efda295159)

### 10. Reviewing Maintenance and Costs
Before I hit the launch button, I took a moment to review everything. I made sure deletion protection was disabled since this was just for testing, and I checked the estimated monthly cost. About $62 USD - not bad at all for what I was getting.

Deletion Protection: **Disabled**  
Estimated Cost: **$61.86 USD**

![J](https://github.com/user-attachments/assets/8805eb8a-fadb-4614-85d6-29448de8365d)

### 11. Watching It Get Created
And we're off! I confirmed everything and AWS started provisioning my database. The status showed **Creating**, so now it was just a waiting game while AWS did its thing in the background.

Status: **Creating**

![K](https://github.com/user-attachments/assets/b873adb2-eda5-49ff-81bd-5eca39a2faa6)

## Part II. Setting Up My Client and Connecting

### 12. Checking My EC2 Command Host
While that was spinning up, I checked on my EC2 instance that I'd be using to connect to the database. I already had one set up called **Command Host** and it was running perfectly with all health checks passing.

Name: **Command Host**  
Status: **Running, 3/3 checks passed**

![L](https://github.com/user-attachments/assets/1dedf686-e5f5-4806-8c1a-0c9876e9b79e)

### 13. Connecting to My EC2 Instance
Time to actually log into that EC2 instance. I used **Session Manager** through the browser - honestly way easier than messing around with SSH keys and all that.

Tool: **Session Manager**

![M](https://github.com/user-attachments/assets/fa2c13ce-a9c8-4f43-aa73-d3e362a8f9a2)

### 14. Installing the Database Client
Once I was logged in, I needed to install the tools that would let me actually talk to my PostgreSQL database. I ran a quick yum install command to grab the MariaDB client package, which works perfectly for connecting to PostgreSQL.

Command: `sudo yum install mariadb -y`

![N](https://github.com/user-attachments/assets/7f45dc29-0cd1-4c86-b073-e2a6b5e67fd8)

### 15. Confirming My Cluster Was Ready
I jumped back over to the RDS console to check if my database had finished provisioning. Success! Both the Aurora cluster and the primary instance showed **Available** status, which meant they were ready to accept connections.

Status: **Available**

![O](https://github.com/user-attachments/assets/05ae5a3b-9ff6-4c07-8b9b-69a938d62c77)

### 16. Finding My Connection Details
I needed to grab the connection endpoints so I'd know where to point my database client. Aurora gives you two endpoints - a **Writer** endpoint for making changes to the database, and a **Reader** endpoint for read-only queries. I made note of both and confirmed the port was **5432** (the standard PostgreSQL port).

Endpoints: **Writer** and **Reader**  
Port: **5432**

![P](https://github.com/user-attachments/assets/d71969ad-5bba-4f9c-a219-58a20411d77f)

## Part III. Working With My Data

### 17. Creating My First Table
Alright, time to actually do something with this database! I wrote up the SQL to create my first table called **country**. I defined all the columns I needed for storing country data and made sure to set up a **PRIMARY KEY** on the Code column so each country would have a unique identifier.

![Q](https://github.com/user-attachments/assets/b619f8f7-011b-4643-9df0-133d24fdb0bb)

### 18. Writing My Query
After getting some data loaded in, I wanted to test out querying it. I wrote a simple SELECT statement to filter countries based on their GNP and population. Specifically, I was looking for countries with a GNP over 35,000 and a population greater than 10 million.

My criteria: `GNP > 35000 AND Population > 10000000`

![R](https://github.com/user-attachments/assets/43567f8f-f0d6-4612-b227-9e05c4e15c40)

### 19. Checking My Results
I ran the query and got back exactly what I was looking for - two countries that matched my criteria: **Australia** and **Thailand**. Perfect! The database was working exactly as expected.

![S](https://github.com/user-attachments/assets/7cbf0c4e-d15d-49f9-9ce1-973bbc359051)

### 20. Lab Complete!
And that's a wrap! I successfully got my Aurora PostgreSQL database deployed, connected to it from my EC2 instance, created a table, loaded some data, and ran queries against it. Everything worked smoothly from start to finish.

**Success:** Aurora instance created, connected, configured, and queried successfully!

![T](https://github.com/user-attachments/assets/1573327e-5ba1-4518-915a-893962a4ec2c)
