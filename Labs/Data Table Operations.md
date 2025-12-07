# AWS EC2 & MariaDB Administration Workflow

## Project Goal

Alright, so this documentation is basically me walking through how I securely accessed an **AWS EC2 instance** and then performed some basic database admin tasks on a **MariaDB server** that was running on that instance. Pretty straightforward stuff, but I wanted to document it properly.

---

## I. AWS EC2 Instance Access (Connection Phase)

This part is all about finding the right EC2 instance and getting connected to it securely using AWS Systems Manager.

### 1. Console Overview and Region Check
First thing I did was make sure I was in the right place. I checked that my **AWS Console Home** was set to the correct region because, trust me, you don't want to be working in the wrong region and wondering why you can't find your resources.

Region confirmed as **us-west-2 (Oregon)**.

![A](https://github.com/user-attachments/assets/761c4a98-8146-4828-a956-7f3f225f1383)

### 2. Navigating the EC2 Dashboard
Once I was sure I was in Oregon, I headed over to the main EC2 Dashboard to get an overview of what was running. I could see all my resources here - running instances, key pairs, volumes, the whole nine yards.

![B](https://github.com/user-attachments/assets/86e0be35-5150-4cef-9b50-59c2580056b0)

### 3. 🎯 Identifying the Target Instance
Now I needed to find the specific EC2 machine I'd be using for this database work. I scrolled through the Instances list and found it - my **Command Host** instance.

Instance Details: Name: **Command Host** (t3.micro) | Status: **Running** | Health: **3/3 checks passed**

![C](https://github.com/user-attachments/assets/797a0576-549a-424d-9aeb-1ddaed28c928)

### 4. Connecting via Session Manager
Time to actually connect to this thing. I went with **Session Manager** for the connection, which is honestly one of the best ways to access EC2 instances. It gives you a browser-based terminal without having to deal with SSH keys or exposing any ports to the internet. Super convenient and more secure too.

![D](https://github.com/user-attachments/assets/100c1020-b79b-4c3d-b085-43367d319d07)

---

## II. MariaDB Database Operations (Administration Phase)

Okay, now that I was connected to the EC2 instance, it was time to get into the database and start working with MariaDB.

### 5. Establishing MariaDB Connection
First thing I did once I was in the terminal was elevate my privileges with sudo su, then launched the MariaDB client using the root user. I had to use the password that was set up for this lab environment.

Command:
bash
mysql -u root --password='rest@rt!'

Success! I was connected to server version **10.5.29-MariaDB**.

![E](https://github.com/user-attachments/assets/413ffdc7-d3eb-4cfe-b1e7-88049bfb0c6d)

### 6. Displaying Initial Databases
Just to see what was already there, I listed out all the default system databases that come with MariaDB right out of the box.

SQL Command:
sql
SHOW DATABASES;

![F](https://github.com/user-attachments/assets/c8a0a852-803f-4f71-b4bb-97b463017a79)

### 7. Creating a New Database
Alright, time to create my own database. I needed a dedicated schema to work with, so I created one called **lab_db**.

SQL Command:
sql
CREATE DATABASE lab_db;

![G](https://github.com/user-attachments/assets/9806e5a3-43dc-419c-b468-7d6a57cebf5e)

### 8. Switching to the New Database
Now that the database existed, I needed to actually switch to it so all my subsequent commands would run in the right context.

SQL Command:
sql
USE lab_db;

![I](https://github.com/user-attachments/assets/0f7974d1-3eed-4992-b648-077b41b5be00)

### 9. Creating a Sample Table
Time to create a table! I designed a simple **inventory** table with an auto-incrementing ID, an item name, and a quantity field. Pretty basic structure, but it's perfect for testing.

SQL Command:
sql
CREATE TABLE inventory (
  id INT NOT NULL AUTO_INCREMENT,
  item_name VARCHAR(100) NOT NULL,
  quantity INT NOT NULL,
  PRIMARY KEY (id)
);

![J](https://github.com/user-attachments/assets/0f3909dd-4a1e-445c-8fc8-2785c74b1cbe)

### 10. Inserting Test Data
Now that I had a table structure, I needed to actually put some data in it. I inserted a test record - just a simple laptop entry with a quantity of 50.

SQL Command:
sql
INSERT INTO inventory (item_name, quantity) 
VALUES ('Laptop', 50);

![K](https://github.com/user-attachments/assets/eaf87729-87ab-41bd-9ea3-aec4773630ab)

### 11. Querying the Data
Time to verify everything worked! I ran a SELECT query to pull up all the data from the inventory table and make sure my record was actually in there.

SQL Command:
sql
SELECT * FROM inventory;

Perfect! There's my laptop entry with an auto-generated ID of 1.

![L](https://github.com/user-attachments/assets/38aeb2db-f201-4f7f-a565-4414291c4c37)

### 12. Exiting the MariaDB Monitor, Workflow Conclusion & Session Termination
All done! I exited the MariaDB session which brought me back to the Linux command prompt, then closed out the Session Manager connection in my browser. That wrapped up everything I needed to do.
SQL Command:
sql
EXIT;

---
