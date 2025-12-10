In this lab, I focused on using Amazon Virtual Private Cloud (VPC) to create my own customized, isolated network environment and then deploying an EC2-based web server into it. This exercise was really cool because it simulated building a real network architecture that you'd use for an enterprise customer.

🎯 What I Aimed to Achieve
My Objectives
By the end of this lab, I wanted to make sure I could:

Create my own virtual private cloud (VPC).
Set up subnets within that VPC.
Configure a security group (basically the firewall rules).
Launch an Amazon EC2 instance into my VPC.

The Scenario
In this lab, I used Amazon VPC to build my own VPC from scratch and added extra components to create a customized network for a Fortune 100 customer. I set up security groups for my EC2 instance, configured a web server, and launched it into the VPC I created.

How I Set Everything Up: VPC and Web Server Deployment
1. 🏗️ Creating My First VPC
I started by setting up my isolated network environment using the automated workflow, which made things pretty straightforward.
1.1 Getting to the VPC Dashboard
First, I opened up the VPC dashboard to kick off the creation process.

<img width="1060" height="226" alt="2" src="https://github.com/user-attachments/assets/7e779119-724d-4e7b-9a6c-ab31fe26c2a5" />

1.2 Defining My VPC Network
I selected the "VPC and more" option because it automatically creates a standard two-tier architecture for me, which saves a lot of time.

<img width="1861" height="882" alt="3" src="https://github.com/user-attachments/assets/e48638cf-bd36-4c8d-ae41-43d72cd16476" />

VPC Name: I called mine Lab VPC.
IPv4 CIDR Block: I set this to 10.0.0.0/16\mathbf{10.0.0.0/16}
10.0.0.0/16, which gives me a large private IP range to work with.

Preview: I could see it would create both Public and Private Subnets along with the associated Route Tables in the us-west-2 region.

1.3 Confirming Everything Got Created
The Create VPC workflow showed me that all my foundational network resources were successfully provisioned, including:

<img width="1847" height="830" alt="4" src="https://github.com/user-attachments/assets/4ef525e5-5562-493a-ac9d-5628408b8ef8" />

My VPC (vpc−0431518c...\mathbf{vpc-0431518c...}
vpc−0431518c...).

DNS hostnames and resolution were enabled.
All the Subnets, Route Tables, and the Internet Gateway (IGW) were created and attached.

1.4 Checking Out My Initial Subnets
Looking at the Subnets dashboard, I could see the four subnets that were automatically created by the workflow.

<img width="1216" height="638" alt="5" src="https://github.com/user-attachments/assets/652e02bb-0e77-49b1-836a-a8782e493696" />

2. 🗺️ Customizing My Subnets and Route Tables
   
<img width="1787" height="743" alt="6" src="https://github.com/user-attachments/assets/77169eb1-ed52-4663-95bd-caa3112be995" />

Next, I customized my network by creating an additional subnet and making sure all my subnets were associated with the correct route tables.
2.1 Adding Another Subnet
I created a new subnet called Public Subnet 2 within my existing Lab VPC (10.0.0.0/16\mathbf{10.0.0.0/16}
10.0.0.0/16).

<img width="1845" height="552" alt="7" src="https://github.com/user-attachments/assets/f18e25ac-fa81-408c-acfc-22a102fcae65" />

2.2 Verifying My New Subnet
The Subnets dashboard confirmed that Public Subnet 2 (subnet−0ada8314...\mathbf{subnet-0ada8314...}
subnet−0ada8314...) was successfully created and available.

<img width="1850" height="831" alt="8" src="https://github.com/user-attachments/assets/ee27e69c-c92d-4ae8-8a07-c883695cdd11" />

2.3 Linking Public Subnets to the Public Route Table
I configured my Public Route Table (rtb-0009f...) to associate with Public Subnet 2 (10.0.2.0/24\mathbf{10.0.2.0/24}
10.0.2.0/24), which ensures it has public access through the IGW.

<img width="1855" height="648" alt="9" src="https://github.com/user-attachments/assets/6540b80e-274f-484c-8fab-759e54c5d0ac" />

2.4 Linking Private Subnets to the Private Route Table
I also configured my Private Route Table (rtb-034fa...) to associate with Private Subnet 2 (10.0.3.0/24\mathbf{10.0.3.0/24}
10.0.3.0/24), which keeps my backend resources isolated as intended.

<img width="1197" height="548" alt="10" src="https://github.com/user-attachments/assets/fc97addf-b10a-4fa7-9d87-829c1790d72b" />

3. 🔒 Setting Up Security and Launching My EC2 Instance
I created a dedicated security group to set up my firewall rules, then deployed my web server instance.
3.1 Starting the Security Group Creation
I went to the Security Groups section and clicked on "Create security group".

<img width="1218" height="616" alt="11" src="https://github.com/user-attachments/assets/4e5b8b28-bf7c-431f-b146-8edecb31cf08" />

3.2 Confirming My Security Group Setup
I successfully created a new Security Group called Web (sg-0f24364f...). Here's what I configured:

<img width="1532" height="666" alt="12 2" src="https://github.com/user-attachments/assets/3909de69-500b-4596-bfa7-9c0f8412ad9a" />

Description: "Enable HTTP access."
Rules: 1 Inbound rule (for HTTP traffic) and 1 Outbound rule.

3.3 Launching My Web Server and Checking Its Health
I launched my Web Server 1 EC2 instance into the public subnet using a t3.micro instance type.

<img width="1246" height="767" alt="12" src="https://github.com/user-attachments/assets/e81b2071-8645-4681-9566-87e43b874142" />

State: Running.
Status Check: Everything looked good with 3/3 checks passed.
IP Addresses: It got assigned a Public IPv4 address (35.91.193.115\mathbf{35.91.193.115}
35.91.193.115) and a
Private IPv4 address (10.0.2.9\mathbf{10.0.2.9}
10.0.2.9).



4. ✅ Testing My Web Server
The final step was confirming that my web server was publicly accessible and wrapping up the lab.
4.1 How I Verified Connectivity
Here's what I did to verify everything was working:
<img width="1246" height="767" alt="12" src="https://github.com/user-attachments/assets/f1eeecb9-496b-48a4-8173-a5c90901ff73" />

I waited until my instance showed 2/2 checks passed.
I copied the Public IPv4 DNS value.
I pasted it into my web browser and hit Enter.

<img width="1241" height="131" alt="13" src="https://github.com/user-attachments/assets/30e6fde8-e0bb-48b9-8c0f-764f4d98191d" />

4.2 Lab Successfully Completed
And that's it! I successfully completed all the objectives for this lab.
