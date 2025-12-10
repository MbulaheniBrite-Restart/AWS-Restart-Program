☁️ My AWS Sales Reporting System: How I Built It Step-by-Step
In this document, I'm walking through how I deployed and configured a fully serverless, automated sales reporting pipeline using AWS services. I'll cover everything from IAM roles to Lambda functions, VPC integration, and scheduling.

Part 1. 🏗️ Getting Started with IAM Role Setup
1. 🔍 Understanding My System Architecture
This diagram shows the complete architecture I built. You can see how data flows from the EventBridge trigger through my Lambda functions and VPC-hosted database to the final SNS topic notification.

<img width="987" height="552" alt="1" src="https://github.com/user-attachments/assets/e5ecb246-cb17-4457-b49c-c0b481a50ff0" />

3. 👤 Creating My IAM Role

<img width="1025" height="728" alt="1 1" src="https://github.com/user-attachments/assets/5f378969-21cc-41bc-af43-53ce8f08450e" />

I started by accessing the IAM console to create the service role I needed. I named this role salesAnalysisReportRole, and it's what gives my Lambda functions permission to interact with other AWS services.
4. 🤝 Setting Up the Trust Policy for Lambda
I set the "Trusted entity type" to AWS service and specified the use case as Lambda. This basically tells AWS that the Lambda service is allowed to assume this role when it's executing my function.

<img width="1037" height="733" alt="2" src="https://github.com/user-attachments/assets/09dc3a50-848f-4a97-a572-5999438c7be9" />
<br></br>
5. ✅ Confirming My Role Was Created
The final screen confirmed that my IAM role was successfully created. I could see all the key details like the Role Name (salesAnalysisReportRole) and the Trust relationship, which verified that the Lambda service was the permitted trusted entity.

<img width="1150" height="740" alt="3" src="https://github.com/user-attachments/assets/61b9bbbf-e96d-4447-88f8-e5529b8ee2e2" />

Part 2. 🧰 Setting Up My Lambda Function and Dependencies
In this section, I deployed my Lambda function and prepared its dependencies using Lambda Layers.
7. 💻 Getting to the Lambda Console
I opened up the AWS Lambda console dashboard—this is where I'd be creating my serverless function.

<img width="1850" height="768" alt="4" src="https://github.com/user-attachments/assets/8bc5fa93-05a9-4f03-9f52-7d9c8155b715" />

8. 📦 Creating a Lambda Layer for My Dependencies
I accessed the Lambda Layers section to create a custom layer. I needed this to package external Python libraries, like the database connector (PyMySQL), so my Lambda function could use them.

<img width="1150" height="732" alt="5" src="https://github.com/user-attachments/assets/eeb93a38-587c-40f7-a8f0-0caf5f5bb899" />

10. ⚙️ Configuring My Layer and Runtime
I configured my new layer and named it pymysqlLibrary. I uploaded the package and made sure it was compatible with Python 3.9, which matches the runtime environment I was using for my Lambda function.


12. <img width="1153" height="736" alt="6" src="https://github.com/user-attachments/assets/041b161a-969d-4e95-b6bc-3f7b8525db9b" />

🚀 Successfully Deploying My Layer
The success screen confirmed that my pymysqlLibrary layer was created with its Amazon Resource Name (ARN). Now the PyMySQL driver was available for my function to import and use.

13. <img width="1138" height="742" alt="7" src="https://github.com/user-attachments/assets/24b1ae69-13b7-49e7-9a1f-3dd3506d38bf" />

➕ Creating My Lambda Function
I started creating my primary data extraction Lambda function, which I named salesAnalysisReportDataExtractor. I used the "Author from scratch" method to build it.

<img width="1151" height="727" alt="8" src="https://github.com/user-attachments/assets/8ad13065-bd4e-485b-aa15-26557d1a9a05" />

14. 📝 Configuring My Function Settings
I configured my function with these key settings: I set the Runtime to Python 3.9 and selected my existing salesAnalysisReportRole as the execution role to give it the initial permissions it needed.

<img width="1156" height="730" alt="9" src="https://github.com/user-attachments/assets/7009ac30-3aad-4423-aa52-00590984485e" />

Part 3. 🌐 Building My VPC and Network Infrastructure
This section covers how I created the dedicated virtual network that my Lambda function needs for private database access.
16. 🏗️ Starting the VPC Creation
Before launching my function, I went to the VPC console to set up my network. I started the VPC creation process using the "VPC and more" option.

<img width="1155" height="737" alt="10" src="https://github.com/user-attachments/assets/85b3daff-ba0b-453c-acfe-5deab9ad306a" />

17. 🏷️ Setting Up My VPC CIDR and Name
I configured my VPC with an IPv4 CIDR block of 10.0.0.0/16 and named it Lab VPC. This defined the primary private IP address range for my network.

<img width="1842" height="748" alt="11" src="https://github.com/user-attachments/assets/780fd0f5-a85d-4c3e-a9bb-265c69d3961d" />

19. 🗺️ Configuring Subnets and Availability Zones
The creation process confirmed that multiple subnets (both public and private) were being provisioned across different Availability Zones. This established the two-tier network design I needed to isolate my database properly.

<img width="1152" height="718" alt="12" src="https://github.com/user-attachments/assets/6fe8d678-e268-4c6e-9abc-3d307b35fa26" />

21. 🚪 Setting Up Gateways
This confirmed my setup included an Internet Gateway (IGW) for public access and a NAT Gateway deployed in a public subnet. The NAT Gateway was crucial because it allows my Lambda function (running in a private subnet) to make outbound connections to AWS services like SNS.

<img width="1150" height="731" alt="13" src="https://github.com/user-attachments/assets/44add9e0-4eaa-44b4-9d20-b960184173c8" />

23. 🧐 Reviewing My VPC Setup
The review screen showed me a summary of all the components that would be created: my VPC, public subnets, private subnets, IGW, and NAT Gateway.

<img width="1155" height="736" alt="14" src="https://github.com/user-attachments/assets/f20eb3cb-f1b8-4c12-bfad-810379ec420d" />

25. ✅ VPC Created Successfully
The success screen confirmed that my VPC was fully created. All the key resources, including my VPC ID (vpc-0431518c...) and subnets, were successfully provisioned.

<img width="1157" height="727" alt="15" src="https://github.com/user-attachments/assets/36a18523-61e9-450b-91d0-80cc0f21d298" />

Part 4. 🔒 Setting Up My EC2 Host and Security Groups
In this section, I set up the hosting environment for my MariaDB database and applied the necessary firewall rules.
27. 🌐 Creating a Security Group for Web Access
I created a new Security Group named Web within my VPC. This group is for internet-facing resources, like the EC2 host that will run my MariaDB database.

<img width="1155" height="725" alt="16" src="https://github.com/user-attachments/assets/030b205c-496b-4584-91dc-276275d0a2d9" />

28. ➡️ Configuring Inbound Rules for Web Security Group
I configured the inbound rules for my Web Security Group to allow the necessary external access: SSH (Port 22), HTTP (Port 80), and HTTPS (Port 443) traffic from anywhere (0.0.0.0/0).

<img width="1155" height="732" alt="17" src="https://github.com/user-attachments/assets/4d59e19c-68e6-49fd-83c3-b58a5a16b2de" />

30. 🛡️ Creating the Database Security Group
I created a second Security Group called Database. This one strictly controls access to my MariaDB instance on port 3306.

<img width="1156" height="672" alt="18" src="https://github.com/user-attachments/assets/052a957c-3539-4e84-b9fe-b9a819f08ab3" />

32. ⛔ Locking Down Database Access (Zero-Trust Approach)
I defined the inbound rule for my Database Security Group to allow MySQL/Aurora (Port 3306) traffic, but I restricted the source to only my Web Security Group's ID (sg-0f243...). This enforces a secure, zero-trust communication path—only the necessary web/DB host and my attached Lambda function can connect.

<img width="1848" height="767" alt="19" src="https://github.com/user-attachments/assets/d42a583d-b8b1-4574-a0fd-7b019522ea85" />

34. 🖥️ Launching My EC2 Instance for the Web Server
I used the EC2 launch wizard to launch an instance named Web Server 1. This instance hosts the MariaDB database that my sales reporting system needs.

<img width="1846" height="771" alt="20" src="https://github.com/user-attachments/assets/f3140ad8-8280-4556-b06c-d1a53b1cd5ec" />

36. ✅ Verifying My EC2 Instance Status
The EC2 dashboard confirmed that my Web Server 1 instance was in the Running state. I could see the Instance ID (i-02f8a74b...) and Public IPv4 address.

<img width="1137" height="665" alt="21" src="https://github.com/user-attachments/assets/70866352-bae6-4ba5-803c-ccc5e80b9b75" />

38. 🛠️ Deploying My CLI Host
I launched a second EC2 instance called CLI Host. This serves as my administrative machine for uploading code, configuring the database, and handling other command-line operations.

<img width="806" height="247" alt="22" src="https://github.com/user-attachments/assets/0eb2f28e-6369-48f5-b3c7-a82559f2ebbd" />

40. ☑️ Confirming CLI Host Status
The EC2 dashboard confirmed that my CLI Host (i-0e022e0c...) was in the Running state and ready for me to use.

<img width="1121" height="646" alt="23" src="https://github.com/user-attachments/assets/0ab9dc9d-8293-420d-a7de-7cb395338a6c" />

Part 5. 🚀 Bringing It All Together: Pipeline Orchestration and Validation
This final section covers how I configured my Lambda function's networking, created the SNS topic, and scheduled the automated report trigger.
42. 📎 Attaching Access Policies to My IAM Role
I went back to my salesAnalysisReportRole IAM role and attached the AmazonSNSFullAccess policy. This gives my Lambda function the permissions it needs to publish messages to my SNS topic.

<img width="1150" height="747" alt="24" src="https://github.com/user-attachments/assets/17a14bf1-5931-458b-b11d-ce6adf53ea75" />

43. 🔗 Connecting My Lambda to the VPC
I modified the network configuration for my Lambda function to connect it to my VPC. I attached it to Private Subnet 1 (subnet-0f46c...) and the Database Security Group (sg-0e360...). This was essential for enabling my Lambda function to establish a private connection to my MariaDB instance.

<img width="1131" height="801" alt="25" src="https://github.com/user-attachments/assets/2a53c811-cde5-4af8-8357-f3515586e8b8" />

45. 📧 Creating My SNS Topic
I created an Amazon Simple Notification Service (SNS) topic and named it salesAnalysisReportTopic. This topic serves as the publication channel for the final sales report I generate.

<img width="1153" height="800" alt="26" src="https://github.com/user-attachments/assets/7754f611-9911-443b-9540-788fc62029e7" />

47. ⏰ Setting Up the EventBridge Scheduler
I configured an EventBridge rule to act as my scheduler. The rule uses a cron expression (cron(0 16 ? * MON-SAT *)) to automatically trigger my Lambda function every day (Monday through Saturday) at 4:00 PM UTC.

<img width="1131" height="737" alt="27" src="https://github.com/user-attachments/assets/e0b141c8-3a6e-4015-b811-c7ff82e200d0" />

49. 💡 Initial Function Configuration
This shows an initial view of my function after creation, highlighting the basic code editor and environment variables section.

<img width="1126" height="737" alt="28" src="https://github.com/user-attachments/assets/b76f1a4c-c76a-4a2d-910b-8a7aa5be2f12" />

51. 🗺️ Reviewing My Lambda VPC Configuration
Here's a summary view of my Lambda function's configuration, showing its successful attachment to the VPC, including the assigned subnets and security groups.

<img width="1165" height="727" alt="29" src="https://github.com/user-attachments/assets/28b85643-4f99-4052-9908-662df8f97940" />

53. 📜 Checking My SNS Topic Access Policy
I reviewed the access policy for my salesAnalysisReportTopic to make sure my salesAnalysisReportRole has the necessary permissions to publish messages to the topic.

<img width="1162" height="727" alt="30" src="https://github.com/user-attachments/assets/2a7b8a72-dc7a-4258-add1-bd39cccc3f3d" />

55. ▶️ Confirming My EventBridge Target
The final EventBridge rule confirmation screen shows the schedule and verifies that the target of my rule is the salesAnalysisReportDataExtractor Lambda function.

<img width="897" height="335" alt="31" src="https://github.com/user-attachments/assets/2794c42e-d059-47c2-b140-d94cee41d340" />

57. ✅ Final Validation: Everything Works!
This final screenshot displays the successful output of my sales report payload. This confirms that my entire automated pipeline—from the scheduled trigger, Lambda execution, data extraction, report formatting, to successful publishing to SNS—is fully operational!

<img width="1166" height="733" alt="32" src="https://github.com/user-attachments/assets/f24c2df3-cc48-4bbf-8999-1dbe6f397dd9" />

<img width="1162" height="727" alt="33" src="https://github.com/user-attachments/assets/b1e04080-ca83-4fe2-a12d-0ae490df7e96" />

