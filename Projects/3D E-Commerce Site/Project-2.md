<img width="688" height="781" alt="2nd Project file" src="https://github.com/user-attachments/assets/258b0f1f-345e-4144-96f0-b74efd9c5878" />

# 🚀 AWS Cloud Architecture Blueprint
## Next-Generation 3D E-Commerce Platform

### 👥 Project Team

| Name |
|------|
| Brite Sendedza |
| Chriswell Maluleke |
| Lindokuhle Dlamini |
| Nayana Mathadeen |
| Onalenna Jack Matlaila |

**📅 Date:** 05 December 2025

---

## 🏗️ The AWS Architecture

<img width="1100" height="850" alt="Ecommerce drawio 1" src="https://github.com/user-attachments/assets/275993b7-6f9b-4f5d-bbe4-e513c48a1cde" />

### 🎯 Why We Chose Each AWS Service

---

### 🎨 **Frontend**

> The frontend refers to the client-side of the application, focusing on user experience and interface. It's what users interact with directly—more like the face of the app.

| Service | Purpose | Simple Analogy |
|---------|---------|----------------|
| **Route 53** | Domain name system (DNS) management for routing users to the application, providing high availability and scalability | 📞 A phonebook of the internet that translates domain names into IP addresses |
| **CloudFront** | Content delivery network (CDN) for distributing static assets, reducing latency, and improving user experience | 🌐 A network of caching servers worldwide storing copies of your website's static files |
| **Amplify Hosting** | Managed hosting for web applications, providing automated deployment, SSL/TLS, and integration with other AWS services | 🏠 A hosting service that handles all technical details automatically |

---

### ⚙️ **Backend**

> The backend refers to the server-side of the application, handling business logic, database interactions, and API integrations. Basically, this is where the magic happens.

| Service | Purpose | Simple Analogy |
|---------|---------|----------------|
| **App Runner** | Containerized application hosting, simplifying deployment and scaling of containerized applications | 📦 A managed platform for running containerized apps without infrastructure worries |
| **Lambda** | Serverless compute for handling small, event-driven tasks, reducing operational overhead | ⚡ A service that runs your code only when needed, no server management required |

---

### 💾 **Storage**

> Storage refers to the services used to store and manage data.

| Service | Purpose | Simple Analogy |
|---------|---------|----------------|
| **S3** (3D assets + static files) | Object storage for hosting static files, providing durability, scalability, and high availability | 🗄️ A massive, secure hard drive in the cloud for storing and serving files |

---

### 🗃️ **Databases**

> Databases are specialized storage services for structured data.

| Service | Use Case | Purpose | Key Features |
|---------|----------|---------|--------------|
| **DynamoDB** | Catalog | NoSQL database for handling high-traffic catalog data | ⚡ Low latency, fast data retrieval |
| **RDS PostgreSQL** | Orders & Users | Relational database for managing orders and user data | 🔒 ACID compliance, SQL querying |

---

### 🚄 **Performance**

> Performance services focus on optimizing application speed and efficiency.

| Service | Purpose | Simple Analogy |
|---------|---------|----------------|
| **ElastiCache Redis** | In-memory caching for improving application performance, reducing database load | 🏃 Super-fast cache layer storing frequently accessed data |
| **SQS + Lambda Workers** | Message queuing and processing for handling asynchronous tasks, decoupling application components | 📬 A message broker handling task queues and processing |

---

### 🔐 **Security**

> Security services protect the application and its data from threats and vulnerabilities.

| Service | Purpose | Simple Analogy |
|---------|---------|----------------|
| **WAF** | Web application firewall protecting against common web exploits and vulnerabilities | 💂 A security guard monitoring and blocking malicious traffic |
| **Shield** | DDoS protection safeguarding against distributed denial-of-service attacks | 🛡️ A protective shield against overwhelming traffic |
| **KMS** | Key management service for encrypting sensitive data and managing encryption keys | 🔑 A secure key vault protecting encryption keys |
| **Secrets Manager** | Securely storing and retrieving sensitive information like database credentials | 🔐 A secure password manager for storing secrets |

---

### 📊 **Monitoring**

> Monitoring services track application performance, errors, and security issues.

| Service | Purpose | Simple Analogy |
|---------|---------|----------------|
| **CloudWatch** | Monitoring and logging service for tracking application performance, errors, and security issues | 📈 A real-time dashboard showing your app's health |
| **Cost Explorer** | Cost management tool for analyzing and optimizing AWS costs | 💰 A financial advisor optimizing AWS spending |

---

## ✅ How Our Architecture Meets the Five Requirements

### 📋 AWS Services Summary

| Category | Services |
|----------|----------|
| **🎨 Frontend** | Route 53, CloudFront, Amplify Hosting |
| **⚙️ Backend** | App Runner, Lambda |
| **💾 Storage** | Amazon S3 (3D Assets + static files) |
| **🗃️ Databases** | DynamoDB, RDS PostgreSQL |
| **🚄 Performance** | ElastiCache Redis, SQS + Lambda |
| **🔐 Security** | WAF, Shield, KMS, Secrets Manager |
| **📊 Monitoring** | CloudWatch, Cost Explorer |

---

### 🎯 Our Five Requirements

1. ⚡ **High Availability**
2. 📈 **Scalability**
3. 🚀 **Performance**
4. 🔒 **Security**
5. 💵 **Cost Optimization**

---

## 📖 How Our Services Meet Our Requirements

### ⚡ **High Availability**

High availability is the ability of a website/service to remain operating for the majority of the time, meaning the website has minimal downtime.

**Key Implementation Strategies:**

- **Elastic Load Balancer (ELB)** directs incoming traffic to healthy endpoints (Lambda, CloudWatch, IP Addresses)
- **CloudFront CDN** caches information at edge locations, making content easily accessible and distributes traffic globally
- **Amazon S3 Multi-AZ** deployment stores assets and static files across multiple availability zones with automatic failover
- **Auto Scaling** increases or decreases compute services according to demand
- **Database Multi-AZ** deployment with failover policy ensures database continuity across availability zones
- **ElastiCache** stores frequently accessed information/files close to customers for faster access

---

### 📈 **Scalability**

With time, demand will increase and the website will need to accommodate more users and a bigger inventory.

**Scalability Solutions:**

- **Amazon S3** provides virtually unlimited storage for growing inventory of static files and 3D assets
- **CloudFront** directs traffic through a global network of servers to increase content delivery
- **Lambda** is a serverless service where AWS manages scaling automatically to meet demand
- **Auto-Scaling** allows dynamic increase or decrease of compute resources based on demand

---

### 🚀 **Performance**

**Performance Optimization Tools:**

| Service | Performance Benefit |
|---------|-------------------|
| **CloudFront** | Caches content closer to users, reducing latency |
| **Elastic Load Balancer** | Distributes traffic to healthy endpoints, re-routing as needed |
| **CloudWatch** | Identifies bottlenecks in the system through monitoring |

---

### 🔒 **Security**

To protect our architecture, we implemented multiple security layers:

**Security Service Details:**

| Service | Protection Type | Key Features |
|---------|----------------|--------------|
| **WAF** | Web Application Firewall | • Filters traffic with allow/block rules<br>• Protects against SQL injection & XSS<br>• Filters by IP, headers, request rate |
| **Shield** | DDoS Protection | • Protects against DDoS attacks<br>• Identifies network security issues |
| **KMS** | Key Management | • Manages cryptographic keys<br>• Handles encryption/decryption |
| **Secrets Manager** | Secrets Protection | • Stores & encrypts API keys, credentials<br>• Manages secret retrieval securely |

---

### 💵 **Cost Optimization**

**AWS Cost Explorer** enables:
- 📊 Visualization of AWS costs and usage
- 🔍 Understanding spending patterns
- ⚙️ Management and optimization of AWS resources
- 💡 Efficient resource utilization strategies

---

## ⚖️ Design Trade-Offs and Challenges

### 1️⃣ **Using App Runner vs. ECS or Lambda for Compute**

| Pros ✅ | Cons ❌ |
|---------|---------|
| Simplified deployment | Reduced infrastructure control |
| Built-in autoscaling | Higher costs for long-running workloads |
| HTTPS out of the box | Less customization options |
| Reduced operational burden | Developer speed prioritized over flexibility |

---

### 2️⃣ **DynamoDB + RDS Combination**

| Pros ✅ | Cons ❌ |
|---------|---------|
| Improved performance for catalog data | Increased operational complexity |
| Better reliability for transactions | Duplicated data models |
| Optimal database per use case | Careful consistency management needed |

---

### 3️⃣ **CloudFront + Amplify Hosting**

| Pros ✅ | Challenges 🔶 |
|---------|---------------|
| Simplified frontend deployment | Managing cache invalidation |
| Global caching for performance | Preventing outdated content display |
| - | Requires cache behavior strategies |

---

### 4️⃣ **High Availability vs. Cost**

**Trade-off Analysis:**

| Improves Reliability ✅ | Increases Cost 💰 |
|------------------------|-------------------|
| Multi-AZ RDS | Significant cost increase |
| Autoscaling compute | Budget constraints |
| Global CDN distribution | Need to balance reliability vs. cost |
| Multiple caching layers | - |

---

### 5️⃣ **Using Redis (ElastiCache) for Performance**

| Benefits ✅ | Challenges 🔶 |
|------------|---------------|
| Very fast response times | Failover complexity |
| Excellent for sessions & carts | In-memory volatility |
| Handles hot data efficiently | Need fallback strategies |

---

### 6️⃣ **Introducing SQS and Lambda Workers**

| Benefits ✅ | Trade-offs ⚖️ |
|------------|---------------|
| Reliable asynchronous processing | Additional moving parts (queues, DLQs) |
| Improved scalability | Complex retry logic |
| - | Must ensure idempotency |
| - | Message integrity critical |

---

### 7️⃣ **Security Layers (WAF, IAM, Shield, KMS)**

| Strengthens Platform 🔒 | Increases Overhead 📊 |
|------------------------|----------------------|
| Comprehensive security | Configuration complexity |
| Multiple defense layers | IAM misconfiguration risks |
| - | WAF rule tuning needed |
| - | KMS API costs |
| - | Requires continuous monitoring |

---

### 8️⃣ **Complexity vs. Simplicity**

**Architecture Characteristics:**

| Highly Scalable & Robust ✅ | Requires Careful Management 🔶 |
|----------------------------|-------------------------------|
| Multiple interconnected services | Strong logging needed |
| Enterprise-grade architecture | Comprehensive monitoring essential |
| Production-ready | Detailed documentation required |
| - | Can be difficult for small teams |

---

## 🎉 Conclusion

This architecture delivers a **highly available**, **scalable**, and **secure** 3D e-commerce platform while maintaining **optimal performance** and **cost efficiency**. The careful selection of AWS services ensures the platform can grow with business demands while maintaining reliability and security standards.

---
