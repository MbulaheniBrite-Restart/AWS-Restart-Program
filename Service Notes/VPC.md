# 🏗️ Build Your VPC and Launch a Web Server

This lab focuses on using **Amazon Virtual Private Cloud (VPC)** to create a customized, isolated network environment within AWS and then deploying an EC2-based web server into that environment. This exercise simulates building a customized network architecture for an enterprise customer.

---

## 🎯 Objectives

After completing this lab, you should be proficient in performing the following foundational AWS networking and compute tasks:

| AWS Component | Action/Skill Gained |
| :--- | :--- |
| **Virtual Private Cloud (VPC)** | Create a fully customized, private virtual network. |
| **Subnets** | Create subnets within the VPC to organize resources (e.g., Public and Private). |
| **Security Group** | Configure a security group to act as a stateful firewall for compute resources. |
| **Amazon EC2** | Launch an **Amazon Elastic Compute Cloud (EC2) instance** into the newly created VPC. |

---

## 🗺️ Scenario: Customized Network Deployment

In this lab, i will use Amazon VPC to create my own VPC and add essential networking components to produce a customized network topology. I will also create security groups to control traffic to your EC2 instance. 

I will then configure and customize an EC2 instance to run a web server and launch it into the VPC, ensuring the final architecture aligns with the following customer diagram:

<img width="1380" height="662" alt="1" src="https://github.com/user-attachments/assets/dbeddbc8-cbef-42ec-b97c-413fdc1bbc30" />


* **Goal:** To build a network environment that supports a publicly accessible web server.
* **Key Tasks:** Setting up the **Internet Gateway** (implied for public access), configuring **Route Tables** for internet-bound traffic, and ensuring the **Security Group** allows inbound web traffic (HTTP/HTTPS).
