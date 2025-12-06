## 🚀 My AWS Sales Reporting System - Service Note

## 📋 Quick Overview

**Project Date:** 11-11-2025 
**Project Type:** Serverless Automated Sales Reporting Pipeline  
**Technologies Used:** Lambda, VPC, RDS, SNS, EventBridge, IAM  
**Built By:** Brite Sendedza  
**Status:** ✅ Fully Operational!

---

## 🎯 What I Set Out to Build

I wanted to create a fully serverless, automated sales reporting pipeline using AWS services. The goal was to have a system that automatically pulls sales data from a database, generates a report, and sends it out via email notification—all without me having to lift a finger once it's set up. Cool, right?

The system needed to run on a schedule, be secure, and be cost-effective. That's the beauty of serverless—I only pay when the function actually runs.

---

## 🔧 The AWS Services I Used (And Why)

### AWS Lambda
**In Simple Terms:** Lambda is basically code that runs in the cloud without me needing to manage any servers. I just write my function, upload it, and AWS handles all the infrastructure.

**How I Used It:** I created a Lambda function called salesAnalysisReportDataExtractor that connects to my database, pulls sales data, formats it into a report, and sends it out. It runs automatically on a schedule.

### AWS IAM (Identity and Access Management)
**In Simple Terms:** IAM is how I control who (or what) can access my AWS resources. It's all about permissions and security.

**How I Used It:** I created a role called salesAnalysisReportRole that gives my Lambda function permission to access the database, send notifications, and do everything else it needs to do.

### Amazon VPC (Virtual Private Cloud)
**In Simple Terms:** VPC is my own private, isolated network in AWS. Think of it like having your own private section of the internet where only your stuff can talk to each other.

**How I Used It:** I set up a VPC to keep my database secure and private. My Lambda function connects to this VPC to access the database without exposing it to the public internet.

### Amazon RDS / MariaDB
**In Simple Terms:** This is my managed database service. Instead of setting up and maintaining my own database server, AWS does the heavy lifting for me.

**How I Used It:** I deployed a MariaDB database on an EC2 instance to store all the sales data that my reporting system needs to access.

### Amazon SNS (Simple Notification Service)
**In Simple Terms:** SNS is a messaging service that can send notifications via email, SMS, or other channels.

**How I Used It:** I created a topic called salesAnalysisReportTopic that my Lambda function publishes to. When the report is ready, SNS sends it out to everyone who's subscribed.

### Amazon EventBridge
**In Simple Terms:** EventBridge is like a smart alarm clock for your AWS resources. You can set up rules to trigger things automatically based on schedules or events.

**How I Used It:** I configured a cron expression to trigger my Lambda function every weekday at 4:00 PM UTC. It's completely hands-off once it's set up.

### Lambda Layers
**In Simple Terms:** Layers let me package up external libraries (like database connectors) and share them across multiple Lambda functions without having to include them in each function's code.

**How I Used It:** I created a layer called pymysqlLibrary that contains the PyMySQL database driver. This keeps my Lambda function code clean and lets me reuse this library in other functions if I need to.

---

## 📝 How I Built Everything (The Full Journey)

### Part 1: Setting Up IAM Roles and Permissions

**Step 1: Understanding the Architecture**
First, I mapped out the entire system architecture. I needed to understand how all the pieces would fit together—from the EventBridge trigger to the Lambda function, through the VPC to the database, and finally to the SNS notification.

**Step 2: Creating My IAM Role**
I went to the IAM console and created a role called salesAnalysisReportRole. This role is what gives my Lambda function the authority to do its job.

**Step 3: Setting Up the Trust Policy**
I configured the trust policy to allow the Lambda service to assume this role. Basically, I'm telling AWS, "Hey, when my Lambda function runs, it's allowed to use these permissions."

**Step 4: Confirming Everything**
I double-checked that the role was created correctly and that the trust relationship was set up properly. Better safe than sorry!

---

### Part 2: Building My Lambda Function

**Step 5: Opening the Lambda Console**
I navigated to the Lambda console where I'd be creating my serverless function. This is where the magic happens!

**Step 6: Creating a Lambda Layer for Dependencies**
Before creating my actual function, I needed to set up a Lambda Layer for the PyMySQL library. This is the database connector that my function needs to talk to MariaDB.

**Step 7: Configuring the Layer**
I named my layer pymysqlLibrary, uploaded the packaged library, and made sure it was compatible with Python 3.9 (the runtime I'd be using).

**Step 8: Layer Successfully Deployed**
The layer was created successfully! Now I had the PyMySQL driver ready to be imported by my function.

**Step 9: Creating My Lambda Function**
Time to create the actual function! I used the "Author from scratch" option and named it salesAnalysisReportDataExtractor.

**Step 10: Configuring Function Settings**
I set the runtime to Python 3.9 and selected my salesAnalysisReportRole as the execution role. This gives my function the permissions it needs right from the start.

---

### Part 3: Building My Secure Network Infrastructure

**Step 11: Starting VPC Creation**
Before my Lambda function could securely access the database, I needed to set up a proper VPC. I used the "VPC and more" option to create everything I needed in one go.

**Step 12: Configuring My VPC**
I set up my VPC with a CIDR block of 10.0.0.0/16 and named it Lab VPC. This gives me plenty of IP addresses to work with.

**Step 13: Setting Up Subnets**
The wizard automatically created both public and private subnets across multiple Availability Zones. This is important for keeping my database isolated while still allowing necessary connections.

**Step 14: Configuring Gateways**
My setup included an Internet Gateway (for public access) and a NAT Gateway (which allows my Lambda function in the private subnet to make outbound connections to AWS services like SNS).

**Step 15: Reviewing Everything**
I reviewed all the components that would be created—VPC, subnets, gateways, route tables—to make sure everything looked good.

**Step 16: VPC Successfully Created**
Success! My VPC was fully provisioned with all the necessary networking components.

---

### Part 4: Setting Up Database Security

**Step 17: Creating the Web Security Group**
I created a security group called Web for internet-facing resources. This would be used by the EC2 instance hosting my MariaDB database.

**Step 18: Configuring Web Access Rules**
I set up inbound rules to allow SSH (Port 22), HTTP (Port 80), and HTTPS (Port 443) traffic. This lets me manage the instance and access the web interface if needed.

**Step 19: Creating the Database Security Group**
I created a second security group specifically called Database to control access to MariaDB on port 3306.

**Step 20: Implementing Zero-Trust Security**
Here's where I got serious about security. I configured the Database security group to ONLY accept connections from the Web security group. This means only my web server and Lambda function (which uses the same security group) can access the database. Nobody else can get in!

**Step 21: Launching My Database Server**
I launched an EC2 instance called Web Server 1 that would host my MariaDB database.

**Step 22: Verifying the Instance**
The instance was up and running! I could see it in the EC2 dashboard with its public IP address.

**Step 23: Deploying My CLI Host**
I also launched a second instance called CLI Host for administrative tasks like uploading code and configuring the database.

**Step 24: Confirming CLI Host Status**
Both instances were now running and ready to go!

---

### Part 5: Bringing Everything Together

**Step 25: Attaching SNS Permissions**
I went back to my IAM role and attached the AmazonSNSFullAccess policy. This allows my Lambda function to publish messages to SNS topics.

**Step 26: Connecting Lambda to the VPC**
I configured my Lambda function to connect to my VPC by attaching it to the private subnet and the Database security group. This is what allows it to securely access my MariaDB database.

**Step 27: Creating My SNS Topic**
I set up an SNS topic called salesAnalysisReportTopic that would be the delivery channel for my reports.

**Step 28: Setting Up the Schedule**
I created an EventBridge rule with a cron expression that triggers my Lambda function Monday through Saturday at 4:00 PM UTC. Completely automated!

**Step 29: Reviewing the Function Configuration**
I took a look at my function's code editor and environment variables to make sure everything was configured properly.

**Step 30: Checking VPC Configuration**
I verified that my Lambda function was correctly attached to the VPC with the right subnets and security groups.

**Step 31: Verifying SNS Permissions**
I reviewed the SNS topic's access policy to ensure my Lambda function had the necessary permissions to publish to it.

**Step 32: Confirming the EventBridge Rule**
I double-checked that my EventBridge rule was correctly configured to trigger my Lambda function on schedule.

**Step 33: Testing Everything!**
The moment of truth! I tested the entire pipeline and got a successful output. The report was generated and published to SNS. Everything worked perfectly!

---

## 🚧 Challenges I Faced

### Understanding Environment Variables in Lambda Functions

**What Happened:** When I was setting up my Lambda function, I kept seeing references to "Environment Variables" and honestly, I wasn't entirely sure what they were or how to use them properly. I could see fields to enter them, but I didn't understand their purpose or how they actually worked within my function.

**Why This Was Confusing:** Coming from a background where I'm used to hardcoding values or passing parameters directly, the concept of environment variables in a serverless context felt a bit abstract. I wasn't sure if they were mandatory, optional, or when I should use them versus other methods of configuration.

---

##  How I Figured It Out

### My Solution: Understanding Environment Variables

After doing some research and experimentation, here's what I learned about environment variables in Lambda functions:

* **What They Actually Are:** Environment variables are basically key-value pairs that you can set for your Lambda function. Think of them like settings or configuration values that your code can access while it's running.

* **Why They're Useful:** Instead of hardcoding things like database connection strings, API keys, or configuration settings directly in my code, I can store them as environment variables. This makes my code more flexible and secure.

* **How They Work in Lambda:** When my Lambda function runs, AWS automatically makes these variables available to my code. In Python, I can access them using os.environ['VARIABLE_NAME']. It's that simple!

* **The Security Benefit:** This is huge—I can store sensitive information like database passwords as environment variables instead of putting them directly in my code. AWS can even encrypt them for extra security.

* **Easy to Update:** If I need to change a database connection string or API endpoint, I don't have to modify and redeploy my code. I just update the environment variable in the Lambda console, and the next time my function runs, it uses the new value.

* **Different Environments:** I can use the same code for development, testing, and production by just changing the environment variables. For example, my dev function can point to a test database while my production function points to the real one.

**Real Example from My Project:** I used environment variables to store my database host, username, and password. My code reads these values at runtime, so I never have to hardcode sensitive credentials. Plus, if I need to change the database server, I just update the environment variable—no code changes needed!

---

## 📊 What I Accomplished

| Component | What I Built | Status |
| :--- | :--- | :--- |
| IAM Role | Created salesAnalysisReportRole with proper permissions |  Complete |
| Lambda Layer | Deployed pymysqlLibrary for database connectivity |  Complete |
| Lambda Function | Built salesAnalysisReportDataExtractor with Python 3.9 |  Complete |
| VPC | Configured Lab VPC with public/private subnets |  Complete |
| Security Groups | Created zero-trust security for database access |  Complete |
| EC2 Instances | Launched database server and CLI host |  Complete |
| SNS Topic | Set up salesAnalysisReportTopic for notifications |  Complete |
| EventBridge Schedule | Automated daily report generation at 4:00 PM UTC |  Complete |
| End-to-End Testing | Validated complete pipeline functionality |  Complete |

---

## 🎓 What I Learned

### The Power of Serverless Architecture
Building this completely serverless system really opened my eyes to how powerful AWS Lambda can be. I don't have to worry about managing servers, scaling, or availability—AWS handles all of that for me. I just focus on the code and the business logic.

### Security Layering is Everything
Setting up the zero-trust security model with security groups taught me a lot about defense in depth. My database isn't directly accessible from the internet—only my Lambda function and web server can reach it, and even then, only through specific paths. That's how real-world systems should be built.

### Environment Variables Make Life Easier
Once I understood how environment variables work, they became one of my favorite features. They make my code so much cleaner and more maintainable. No more hardcoded secrets!

### VPC Networking Matters
Understanding how VPCs, subnets, gateways, and security groups work together was crucial for this project. The networking layer is what enables secure communication between all the components.

### Automation is Incredible
Setting up EventBridge to automatically trigger my function on a schedule means this entire system now runs itself. Once deployed, it just works—day after day, generating reports without me having to do anything.

---

##  What I'd Do Next

* **Add Error Handling:** Implement more robust error handling and retry logic in the Lambda function
* **Set Up CloudWatch Alarms:** Get notified if the function fails or if there are any issues with the pipeline
* **Implement Report Archiving:** Store historical reports in S3 for future reference
* **Add Data Validation:** Include checks to ensure the data quality before generating reports
* **Cost Optimization:** Review and optimize the function's memory allocation and timeout settings
* **Add More Recipients:** Configure SNS to send reports to multiple stakeholders

---

## 📎 What I Referenced

* AWS Lambda Documentation: https://docs.aws.amazon.com/whitepapers/latest/serverless-multi-tier-architectures-api-gateway-lambda/aws-lambda.html
* AWS VPC Best Practices: https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
* EventBridge Cron Expressions: https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html

---

## ✍️ Final Thoughts

**Project Status:**  Successfully deployed and operational  
**Confidence Level:**  High—I can now build serverless pipelines independently  
**Would I Do This Again?** Absolutely! This is exactly the kind of automation businesses need

**My Closing Notes:** This project was incredibly rewarding. I built a completely automated, serverless sales reporting system from scratch using multiple AWS services working together. The best part? Once it's deployed, it just runs itself—pulling data, generating reports, and sending notifications every single day without any manual intervention. 

Understanding environment variables was a key breakthrough for me, and the zero-trust security model I implemented gives me confidence that the system is secure. This is the kind of real-world, production-ready solution that companies actually use, and I'm proud to have built it myself.

**Completed On:** 11-11-2025  
**Built By:** Brite Sendedza
