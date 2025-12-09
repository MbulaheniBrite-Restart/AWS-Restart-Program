# 🔥 Embers & Co. Digital Experience: Serverless Booking Platform

**Tagline:** *"Crafted with FIRE, Served with SOUL"*

Welcome to the technical documentation for the **Embers & Co. Digital Experience** project. This project is a robust, cost-effective solution designed to transform a manual restaurant operation into a seamless, high-efficiency digital platform using Amazon Web Services (AWS).

---

## 1. 🚀 Project Summary

### The Challenge

Embers & Co. faced significant operational friction: manual order/booking systems led to double-bookings, lost reservations, and wasted staff time. The goal was to eliminate this "Chaos of Success" and establish a stable digital foundation.

### The Solution

A modern, fully **Serverless** application hosted in the **AWS Cape Town Region**. The architecture provides a lightning-fast static website and a zero-error backend for booking and order submissions.

| Metric | Pre-Migration Status | **Post-Migration Result** |
| :--- | :--- | :--- |
| **Error Rate** | High (Lost Revenue & Confusion) | **Zero** (100% Precision Capture) |
| **Operational Visibility** | Blind Decisions (Lack of Central Data) | **Real-Time** (Secure DynamoDB Storage) |
| **Scalability** | Costly & Time-Consuming | **Infinite** (Pay-per-Use Model) |

---

## 2. 🏛️ Solution Architecture Overview

This project utilizes a resilient, serverless stack to ensure high availability, security, and low operational cost.

### Key Components

The system is split into two primary layers: **Fast Content Delivery** and **Dynamic Booking Logic**.

| Service | Category | Function in Project |
| :--- | :--- | :--- |
| **Amazon S3** | Storage | Durable hosting for all static website assets (`index.html`, CSS, JS, images). |
| **Amazon CloudFront** | CDN | Caches content globally for **lightning-fast loading** and provides a crucial layer of **DDoS protection**. |
| **Amazon API Gateway** | API Management | The secure, external entry point for all dynamic customer requests (e.g., submitting a booking form). |
| **AWS Lambda** | Compute | The serverless "Brain" that processes booking logic, validates data, and sends notifications. **(Runs only when needed)**. |
| **Amazon DynamoDB** | Database | A fully managed NoSQL database for **securely storing** all customer and reservation data. |
| **Amazon SES** | Communication | Handles automated, instant **confirmation emails and SMS alerts** to guests. |

### Architecture Diagram

The diagram below illustrates the flow of data, from the user's browser through the front-end delivery and into the dynamic backend logic.



---

## 3. ⚙️ Booking Process Flow

The **Zero-Error Booking** process is the core engine that handles reservations reliably:

1.  **Submission:** Customer fills out the booking form on the S3/CloudFront-hosted static website.
2.  **Routing:** The request is securely routed via **API Gateway**.
3.  **Execution:** **AWS Lambda** processes the booking request and validates the data.
4.  **Storage:** The reservation details are immediately stored in **DynamoDB**.
5.  **Confirmation:** **Amazon SES** triggers the confirmation email, and a success response is sent back to the user's browser.

---

## 4. 💰 Cost & Investment Summary

This architecture provides **Enterprise-Grade Technology** at a local price point, transforming operations for a minimal infrastructure cost.

> 💡 **Cost Efficiency:** The heavy use of the AWS Free Tier for services like Lambda, DynamoDB, and SES ensures a nearly zero infrastructure cost at launch.

| Service Component | Estimated Monthly Cost | Detail |
| :--- | :--- | :--- |
| **S3, CloudFront & Route 53** | ~\$9.64 USD (~R165) | Covers storage, low traffic data transfer, and DNS hosting. |
| **API Gateway** | \$0 USD | Free Tier covers 1M requests/month. |
| **AWS Lambda Functions** | \$0 USD | Free Tier covers 1M requests/month. |
| **DynamoDB** | \$0 USD | Free Tier covers 25GB storage. |
| **Amazon SES** | \$0 USD | Free Tier covers 3,000 messages/month (for 12 months). |
| **ACM (SSL/TLS)** | \$0 USD | Always Free. |
| **TOTAL ESTIMATED INFRASTRUCTURE** | **~ \$9.64 USD / Month** | **Less than the cost of two gourmet meals.** |

---

## 5. 🗺️ Roadmap to Go-Live

The successful deployment followed a structured, 7-week plan to ensure a smooth transition with zero downtime:

| Week(s) | Phase | Key Activities |
| :--- | :--- | :--- |
| **Wk 1-2** | **Plan & Finalize** | Review current data, finalize serverless architecture, and set success metrics. |
| **Wk 3-4** | **Build Front-End** | Deploy the static website prototype, S3 bucket, CloudFront distribution, and Route 53 setup. |
| **Wk 5-6** | **Integrate & Test** | Connect booking forms to the API Gateway/Lambda/DynamoDB/SES backend stack. |
| **Wk 7** | **Go-Live** | **Zero-downtime migration** and comprehensive staff training on the new digital system. |

***

*This project was completed as Project 1 of the Praesignis AWS re/Start Projects program, demonstrating proficiency in scalable cloud architecture.*
