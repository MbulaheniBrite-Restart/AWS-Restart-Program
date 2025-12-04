☁️ My AWS Sales Reporting System: How I Built It Step-by-Step
In this document, I'm walking through how I deployed and configured a fully serverless, automated sales reporting pipeline using AWS services. I'll cover everything from IAM roles to Lambda functions, VPC integration, and scheduling.

Part 1. 🏗️ Getting Started with IAM Role Setup
1. 🔍 Understanding My System Architecture
This diagram shows the complete architecture I built. You can see how data flows from the EventBridge trigger through my Lambda functions and VPC-hosted database to the final SNS topic notification.
<img width="987" height="552" alt="1" src="https://github.com/user-attachments/assets/9b118b3b-270d-4caa-bc1e-cf3b8e48063b" />
2. 👤 Creating My IAM Role
<img width="1025" height="728" alt="1 1" src="https://github.com/user-attachments/assets/707706d0-de56-4c20-8c90-13f5cbd7ebe4" />
I started by accessing the IAM console to create the service role I needed. I named this role salesAnalysisReportRole, and it's what gives my Lambda functions permission to interact with other AWS services.
3. 🤝 Setting Up the Trust Policy for Lambda
I set the "Trusted entity type" to AWS service and specified the use case as Lambda. This basically tells AWS that the Lambda service is allowed to assume this role when it's executing my function.
<img width="1150" height="740" alt="3" src="https://github.com/user-attachments/assets/1a593412-0492-4ced-84fb-b333818cb151" />
4. ✅ Confirming My Role Was Created
The final screen confirmed that my IAM role was successfully created. I could see all the key details like the Role Name (salesAnalysisReportRole) and the Trust relationship, which verified that the Lambda service was the permitted trusted entity.
<img width="1153" height="736" alt="6" src="https://github.com/user-attachments/assets/c18f7689-f19f-44e6-a080-1913d531c7e8" />
Part 2. 🧰 Setting Up My Lambda Function and Dependencies
In this section, I deployed my Lambda function and prepared its dependencies using Lambda Layers.
5. 💻 Getting to the Lambda Console
I opened up the AWS Lambda console dashboard—this is where I'd be creating my serverless function.
<img width="1150" height="732" alt="5" src="https://github.com/user-attachments/assets/255f3520-3d28-4957-90dd-02bdf307fd17" />
6. 📦 Creating a Lambda Layer for My Dependencies
I accessed the Lambda Layers section to create a custom layer. I needed this to package external Python libraries, like the database connector (PyMySQL), so my Lambda function could use them.
<img width="1153" height="736" alt="6" src="https://github.com/user-attachments/assets/851a5b8c-5fd4-4941-9727-baa8226c60a6" />
7. ⚙️ Configuring My Layer and Runtime
I configured my new layer and named it pymysqlLibrary. I uploaded the package and made sure it was compatible with Python 3.9, which matches the runtime environment I was using for my Lambda function.
<img width="1138" height="742" alt="7" src="https://github.com/user-attachments/assets/0a6ab24b-fc0c-4ff7-98ba-9fd73593937e" />
8. 🚀 Successfully Deploying My Layer
The success screen confirmed that my pymysqlLibrary layer was created with its Amazon Resource Name (ARN). Now the PyMySQL driver was available for my function to import and use.
<img width="1151" height="727" alt="8" src="https://github.com/user-attachments/assets/6feaacf5-1978-44fb-babb-37dbe45feff3" />
9. ➕ Creating My Lambda Function
I started creating my primary data extraction Lambda function, which I named salesAnalysisReportDataExtractor. I used the "Author from scratch" method to build it.
<img width="1156" height="730" alt="9" src="https://github.com/user-attachments/assets/265c90fa-76c8-43e2-b266-7fb70bdfc3d6" />
10. 📝 Configuring My Function Settings
I configured my function with these key settings: I set the Runtime to Python 3.9 and selected my existing salesAnalysisReportRole as the execution role to give it the initial permissions it needed.
<img width="1155" height="737" alt="10" src="https://github.com/user-attachments/assets/e9b82a0a-1c83-45a9-b5b2-911e04f34fb9" />
Part 3. 🌐 Building My VPC and Network Infrastructure
This section covers how I created the dedicated virtual network that my Lambda function needs for private database access.
11. 🏗️ Starting the VPC Creation
Before launching my function, I went to the VPC console to set up my network. I started the VPC creation process using the "VPC and more" option.
<img width="1156" height="672" alt="18" src="https://github.com/user-attachments/assets/783dfccc-ad9b-40fc-beb3-a2a6f147aebb" />
12. 🏷️ Setting Up My VPC CIDR and Name
I configured my VPC with an IPv4 CIDR block of 10.0.0.0/16 and named it Lab VPC. This defined the primary private IP address range for my network.
<img width="1848" height="767" alt="19" src="https://github.com/user-attachments/assets/68c38df6-ed1a-469c-ba4e-ecbc9cbed92d" />
13. 🗺️ Configuring Subnets and Availability Zones
The creation process confirmed that multiple subnets (both public and private) were being provisioned across different Availability Zones. This established the two-tier network design I needed to isolate my database properly.
<img width="1150" height="731" alt="13" src="https://github.com/user-attachments/assets/3feb7810-d6e2-41a3-9a2a-5ba40d04f4af" />
14. 🚪 Setting Up Gateways
This confirmed my setup included an Internet Gateway (IGW) for public access and a NAT Gateway deployed in a public subnet. The NAT Gateway was crucial because it allows my Lambda function (running in a private subnet) to make outbound connections to AWS services like SNS.
<img width="1155" height="736" alt="14" src="https://github.com/user-attachments/assets/2f109dc6-c855-4e44-8924-9789b2de53cb" />
15. 🧐 Reviewing My VPC Setup
The review screen showed me a summary of all the components that would be created: my VPC, public subnets, private subnets, IGW, and NAT Gateway.
<img width="1157" height="727" alt="15" src="https://github.com/user-attachments/assets/a32e9206-836d-402e-abbf-3d72f5be811f" />
16. ✅ VPC Created Successfully
The success screen confirmed that my VPC was fully created. All the key resources, including my VPC ID (vpc-0431518c...) and subnets, were successfully provisioned.
<img width="1155" height="725" alt="16" src="https://github.com/user-attachments/assets/a4dd3e08-d951-460d-88d7-267faed82864" />
Part 4. 🔒 Setting Up My EC2 Host and Security Groups
In this section, I set up the hosting environment for my MariaDB database and applied the necessary firewall rules.
17. 🌐 Creating a Security Group for Web Access
I created a new Security Group named Web within my VPC. This group is for internet-facing resources, like the EC2 host that will run my MariaDB database.
<img width="1155" height="732" alt="17" src="https://github.com/user-attachments/assets/2d9b4880-b643-4a18-97a5-f3bcc0b14d2e" />
18. ➡️ Configuring Inbound Rules for Web Security Group
I configured the inbound rules for my Web Security Group to allow the necessary external access: SSH (Port 22), HTTP (Port 80), and HTTPS (Port 443) traffic from anywhere (0.0.0.0/0).
<img width="1156" height="672" alt="18" src="https://github.com/user-attachments/assets/9e6299db-e7b9-4b9d-875e-34788922b02e" />
19. 🛡️ Creating the Database Security Group
I created a second Security Group called Database. This one strictly controls access to my MariaDB instance on port 3306.
<img width="1848" height="767" alt="19" src="https://github.com/user-attachments/assets/2107f117-244c-428f-9a4b-1965a0869956" />
20. ⛔ Locking Down Database Access (Zero-Trust Approach)
I defined the inbound rule for my Database Security Group to allow MySQL/Aurora (Port 3306) traffic, but I restricted the source to only my Web Security Group's ID (sg-0f243...). This enforces a secure, zero-trust communication path—only the necessary web/DB host and my attached Lambda function can connect.
<img width="1846" height="771" alt="20" src="https://github.com/user-attachments/assets/44fcc952-a2df-4ad4-bf23-743a0d2fa8ea" />
21. 🖥️ Launching My EC2 Instance for the Web Server
I used the EC2 launch wizard to launch an instance named Web Server 1. This instance hosts the MariaDB database that my sales reporting system needs.
<img width="1137" height="665" alt="21" src="https://github.com/user-attachments/assets/3201140e-4cdd-4a6f-82e1-18dd41a98401" />
22. ✅ Verifying My EC2 Instance Status
The EC2 dashboard confirmed that my Web Server 1 instance was in the Running state. I could see the Instance ID (i-02f8a74b...) and Public IPv4 address.
<img width="806" height="247" alt="22" src="https://github.com/user-attachments/assets/5e2642fc-9f2e-475c-8a14-af5ebc437d18" />
23. 🛠️ Deploying My CLI Host
I launched a second EC2 instance called CLI Host. This serves as my administrative machine for uploading code, configuring the database, and handling other command-line operations.
<img width="1121" height="646" alt="23" src="https://github.com/user-attachments/assets/7dd8bc06-c2b0-4926-b6cb-bfd483bb8c5f" />
24. ☑️ Confirming CLI Host Status
The EC2 dashboard confirmed that my CLI Host (i-0e022e0c...) was in the Running state and ready for me to use.
<img width="1150" height="747" alt="24" src="https://github.com/user-attachments/assets/a66b384e-9cde-4f31-9754-cc0ccbab9acc" />
Part 5. 🚀 Bringing It All Together: Pipeline Orchestration and Validation
This final section covers how I configured my Lambda function's networking, created the SNS topic, and scheduled the automated report trigger.
25. 📎 Attaching Access Policies to My IAM Role
I went back to my salesAnalysisReportRole IAM role and attached the AmazonSNSFullAccess policy. This gives my Lambda function the permissions it needs to publish messages to my SNS topic.
<img width="1131" height="801" alt="25" src="https://github.com/user-attachments/assets/318ce7ab-2286-4473-be1f-a48d743dd99c" />
26. 🔗 Connecting My Lambda to the VPC
I modified the network configuration for my Lambda function to connect it to my VPC. I attached it to Private Subnet 1 (subnet-0f46c...) and the Database Security Group (sg-0e360...). This was essential for enabling my Lambda function to establish a private connection to my MariaDB instance.
<img width="1153" height="800" alt="26" src="https://github.com/user-attachments/assets/bdc8fc22-6abf-454d-b04e-e993511dadb5" />
27. 📧 Creating My SNS Topic
I created an Amazon Simple Notification Service (SNS) topic and named it salesAnalysisReportTopic. This topic serves as the publication channel for the final sales report I generate.
<img width="1131" height="737" alt="27" src="https://github.com/user-attachments/assets/2aef00e9-b52a-483b-9cda-333014745992" />
28. ⏰ Setting Up the EventBridge Scheduler
I configured an EventBridge rule to act as my scheduler. The rule uses a cron expression (cron(0 16 ? * MON-SAT *)) to automatically trigger my Lambda function every day (Monday through Saturday) at 4:00 PM UTC.
<img width="1126" height="737" alt="28" src="https://github.com/user-attachments/assets/cf50951f-746f-4e2d-9d60-d420d797423a" />
29. 💡 Initial Function Configuration
This shows an initial view of my function after creation, highlighting the basic code editor and environment variables section.
<img width="1165" height="727" alt="29" src="https://github.com/user-attachments/assets/ce6b6c0b-75b1-4d08-bd8f-7e13836b3f66" />
30. 🗺️ Reviewing My Lambda VPC Configuration
Here's a summary view of my Lambda function's configuration, showing its successful attachment to the VPC, including the assigned subnets and security groups.
<img width="1162" height="727" alt="30" src="https://github.com/user-attachments/assets/9497d66f-8b5d-434f-9976-b82cb182e5bf" />
31. 📜 Checking My SNS Topic Access Policy
I reviewed the access policy for my salesAnalysisReportTopic to make sure my salesAnalysisReportRole has the necessary permissions to publish messages to the topic.
<img width="897" height="335" alt="31" src="https://github.com/user-attachments/assets/0435995c-e827-4daf-ab17-1430529d2d74" />
32. ▶️ Confirming My EventBridge Target
The final EventBridge rule confirmation screen shows the schedule and verifies that the target of my rule is the salesAnalysisReportDataExtractor Lambda function.
<img width="1817" height="698" alt="32" src="https://github.com/user-attachments/assets/a79e0b31-098e-4ea4-8634-bbed509e9e6e" />
33. ✅ Final Validation: Everything Works!
This final screenshot displays the successful output of my sales report payload. This confirms that my entire automated pipeline—from the scheduled trigger, Lambda execution, data extraction, report formatting, to successful publishing to SNS—is fully operational!
<img width="1162" height="727" alt="33" src="https://github.com/user-attachments/assets/0c2e36f4-a834-4d04-b8ac-17a7a618ec21" />
