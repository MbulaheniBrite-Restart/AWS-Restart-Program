🛡️ My AWS EC2 Instance Management Guide: Deploying a Web Server
In this document, I'm walking through how I launched, managed, and secured an Amazon EC2 web server instance. I'll cover everything from its lifecycle to termination protection and resizing.

<img width="1726" height="651" alt="1" src="https://github.com/user-attachments/assets/10553e6b-8353-48b5-aa4b-eff033f619e9" />
1. 🌐 Getting to the EC2 Dashboard
First thing I did was navigate to the AWS Management Console and made sure I was in the right region (I used US West - Oregon for this).

What I did: I found the EC2 service either in the "Recently visited" section or by using the services search bar.
Why this matters: The EC2 Dashboard is basically my central hub for launching instances, configuring security groups, and managing storage volumes.


<img width="1831" height="700" alt="2" src="https://github.com/user-attachments/assets/d644c9de-7bb5-4467-b711-aa4eb5f41d28" />
2. 🖥️ Naming My Instance and Choosing the OS
I started the "Launch instances" wizard to set up my instance's identity and pick the operating system.
What I ConfiguredWhat I ChoseWhy I Chose ItName and TagsWeb ServerI gave it a descriptive name so I could easily identify it later.Application and OS ImageAmazon Linux 2023 AMIThis gives me a stable, secure, and high-performance environment to work with.

<img width="1848" height="762" alt="3" src="https://github.com/user-attachments/assets/e68ab0f3-cbc3-4410-b591-fcb882c67d94" />
3. 🔒 Setting Up Network and Security Groups
This part was really important because I needed to define my firewall rules and how my instance would communicate with the internet.

VPC & Subnet: I made sure to select the correct VPC and Subnet for my deployment.
Security Group: I clicked on "Create security group" and named it Web Server Security group.
Firewall Rule: This was crucial—I needed to set up inbound rules to allow HTTP traffic so people could actually reach my web server.


<img width="1817" height="698" alt="4" src="https://github.com/user-attachments/assets/a79e0b31-098e-4ea4-8634-bbed509e9e6e" />
4. ⚙️ Automating the Setup with User Data (Bootstrapping)
To automate the web server installation, I used the "User data" field in the Advanced Details section. This is pretty cool because it runs scripts automatically when the instance starts.
Here's the Bash Script I Used:
🛡️ My AWS EC2 Instance Management Guide: Deploying a Web Server
In this document, I'm walking through how I launched, managed, and secured an Amazon EC2 web server instance. I'll cover everything from its lifecycle to termination protection and resizing.

<img width="1726" height="651" alt="1" src="https://github.com/user-attachments/assets/10553e6b-8353-48b5-aa4b-eff033f619e9" />
1. 🌐 Getting to the EC2 Dashboard
First thing I did was navigate to the AWS Management Console and made sure I was in the right region (I used US West - Oregon for this).

What I did: I found the EC2 service either in the "Recently visited" section or by using the services search bar.
Why this matters: The EC2 Dashboard is basically my central hub for launching instances, configuring security groups, and managing storage volumes.


<img width="1831" height="700" alt="2" src="https://github.com/user-attachments/assets/d644c9de-7bb5-4467-b711-aa4eb5f41d28" />
2. 🖥️ Naming My Instance and Choosing the OS
I started the "Launch instances" wizard to set up my instance's identity and pick the operating system.
What I ConfiguredWhat I ChoseWhy I Chose ItName and TagsWeb ServerI gave it a descriptive name so I could easily identify it later.Application and OS ImageAmazon Linux 2023 AMIThis gives me a stable, secure, and high-performance environment to work with.

<img width="1848" height="762" alt="3" src="https://github.com/user-attachments/assets/e68ab0f3-cbc3-4410-b591-fcb882c67d94" />
3. 🔒 Setting Up Network and Security Groups
This part was really important because I needed to define my firewall rules and how my instance would communicate with the internet.

VPC & Subnet: I made sure to select the correct VPC and Subnet for my deployment.
Security Group: I clicked on "Create security group" and named it Web Server Security group.
Firewall Rule: This was crucial—I needed to set up inbound rules to allow HTTP traffic so people could actually reach my web server.


<img width="1817" height="698" alt="4" src="https://github.com/user-attachments/assets/a79e0b31-098e-4ea4-8634-bbed509e9e6e" />
4. ⚙️ Automating the Setup with User Data (Bootstrapping)
To automate the web server installation, I used the "User data" field in the Advanced Details section. This is pretty cool because it runs scripts automatically when the instance starts.

<br></br>
5. ✅ Keeping an Eye on My Instance Status
After launching, I needed to monitor my instance to make sure everything was working properly.
What I CheckedWhat I Wanted to SeeWhat It MeansInstance StateRunningMy instance successfully booted up after being in the Pending state.Status check3/3 checks passedThis confirmed that both the system reachability and instance connectivity were healthy, and my server was ready to accept traffic.
<img width="1852" height="763" alt="6" src="https://github.com/user-attachments/assets/f107d5c3-c420-47c0-b75f-1e5f713f944a" />

<br></br>
6. ⬆️ Resizing My Instance (Vertical Scaling)
When I needed to change the instance type (resize it), I had to stop the instance first so I could modify the compute resources.
StateHow I Did ItWhyExampleStoppedI went to the Actions menu.I needed to do this before I could modify any hardware settings.N/ASettingsI clicked Instance settings →\rightarrow
→
Change instance type.This let me scale my server (like going from t3.micro to t3.small) to handle more load.Scaling Up

<br></br>
<img width="1548" height="405" alt="7" src="https://github.com/user-attachments/assets/c6e38fb0-49fb-4aaf-bec2-f09a258d79b5" />
7. 🔒 Testing the Termination Protection
Termination protection is super important because it prevents me from accidentally deleting critical resources. I tested this to make sure it was working.

What I Tried: I attempted to "Terminate (delete) instance."
What Happened: The console gave me an error message, which actually confirmed that my security configuration was working properly and blocked the action:


Error Message: "Failed to terminate... Modify its 'disableApiTermination' instance attribute and try again."
<img width="1852" height="763" alt="6" src="https://github.com/user-attachments/assets/f107d5c3-c420-47c0-b75f-1e5f713f944a" />
