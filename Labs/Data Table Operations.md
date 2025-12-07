## AWS EC2 & MariaDB Administration Workflow

## Project Goal

This documentation provides a comprehensive, step-by-step workflow detailing the secure access of a running **AWS EC2 instance** and the execution of fundamental database administration tasks on a **MariaDB server** hosted on that instance.

---

## I. AWS EC2 Instance Access (Connection Phase)

This section covers the process of identifying the target EC2 resource and initiating a secure, private connection using AWS Systems Manager.

### 1. Console Overview and Region Check
* **Description:** Verified the correct console context, ensuring the **AWS Console Home** was set to the operational region.
* **Key Detail:** Region confirmed as **us-west-2 (Oregon)**.

<img width="920" height="355" alt="A" src="https://github.com/user-attachments/assets/761c4a98-8146-4828-a956-7f3f225f1383" />

### 2. Navigating the EC2 Dashboard
* **Description:** Accessed the primary EC2 Dashboard to gain a regional overview of all running resources.
* **Key Insight:** Reviewed resource categories including **Instances (running)**, **Key pairs**, and **Volumes**.

<img width="920" height="359" alt="B" src="https://github.com/user-attachments/assets/86e0be35-5150-4cef-9b50-59c2580056b0" />

### 3. 🎯 Identifying the Target Instance
* **Description:** Located the specific EC2 machine required for the database administration task within the Instances list.
* **Instance Details:** Name: **Command Host** (t3.micro) | Status: **Running** | Health: **3/3 checks passed**.

<img width="920" height="353" alt="C" src="https://github.com/user-attachments/assets/797a0576-549a-424d-9aeb-1ddaed28c928" />

### 4. Connecting via Session Manager
* **Description:** Selected the **Session Manager** connection method, which establishes a secure, browser-based terminal session without exposing public inbound ports or requiring SSH keys.

<img width="920" height="353" alt="D" src="https://github.com/user-attachments/assets/100c1020-b79b-4c3d-b085-43367d319d07" />

---

## II. MariaDB Database Operations (Administration Phase)

This section documents the specific terminal commands executed on the connected EC2 instance to perform database initialization and data manipulation.

### 5. Establishing MariaDB Connection
* **Description:** Elevated shell privileges (sudo su) and launched the MariaDB client using the root user credentials.
* **Command:**
    bash
    mysql -u root --password='rest@rt!'
    
* **Verification:** Confirmed the connection to server version **10.5.29-MariaDB**.

<img width="782" height="339" alt="E" src="https://github.com/user-attachments/assets/413ffdc7-d3eb-4cfe-b1e7-88049bfb0c6d" />

### 6. Displaying Initial Databases
* **Description:** Listed the default system databases present immediately after connecting to the MariaDB monitor.
* **SQL Command:**
    sql
    SHOW DATABASES;
    
<img width="907" height="364" alt="F" src="https://github.com/user-attachments/assets/c8a0a852-803f-4f71-b4bb-97b463017a79" />

### 7. Creating a New Database
* **Description:** Created a dedicated, new database schema to host the application data.
* **SQL Command:**
    sql
    CREATE DATABASE lab_db;
    
<img width="916" height="377" alt="G" src="https://github.com/user-attachments/assets/9806e5a3-43dc-419c-b468-7d6a57cebf5e" />

### 8. Switching to the New Database
* **Description:** Set **lab_db** as the active database context for all subsequent table and data operations.
* **SQL Command:**
    sql
    USE lab_db;
    
<img width="836" height="376" alt="I" src="https://github.com/user-attachments/assets/0f7974d1-3eed-4992-b648-077b41b5be00" />

### 9. Creating a Sample Table
* **Description:** Defined the structure of the **inventory** table with necessary columns (id, item_name, quantity) and data types.
* **SQL Command:**
    sql
    CREATE TABLE inventory (
      id INT NOT NULL AUTO_INCREMENT,
      item_name VARCHAR(100) NOT NULL,
      quantity INT NOT NULL,
      PRIMARY KEY (id)
    );
    
<img width="914" height="377" alt="J" src="https://github.com/user-attachments/assets/0f3909dd-4a1e-445c-8fc8-2785c74b1cbe" />

### 10. Inserting Test Data
* **Description:** Inserted a test record into the newly created **inventory** table to populate it with initial data.
* **SQL Command:**
    sql
    INSERT INTO inventory (item_name, quantity) 
    VALUES ('Laptop', 50);
    
<img width="677" height="208" alt="K" src="https://github.com/user-attachments/assets/eaf87729-87ab-41bd-9ea3-aec4773630ab" />

### 11. Querying the Data
* **Description:** Executed a SELECT query to verify the successful data insertion and display the table's contents.
* **SQL Command:**
    sql
    SELECT * FROM inventory;

    <img width="799" height="188" alt="L" src="https://github.com/user-attachments/assets/38aeb2db-f201-4f7f-a565-4414291c4c37" />

### 12. Exiting the MariaDB Monitor, Workflow Conclusion & Session Termination
* **Description:** Gracefully closed the interactive MariaDB session, returning the user to the Linux command prompt.
* **Description:** The browser-based Session Manager connection was closed, terminating the link to the EC2 instance and concluding the documented administrative workflow.
* **SQL Command:**
    sql
    EXIT;
  ---
