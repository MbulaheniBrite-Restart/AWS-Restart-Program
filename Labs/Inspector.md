## 🔒 AWS Security Assessment: Amazon Inspector Workflow

## Project Goal

So here's what I set out to do - I wanted to get **Amazon Inspector** up and running to perform continuous, automated vulnerability scanning on my AWS resources. Specifically, I focused on scanning **AWS Lambda functions** for security vulnerabilities and then actually fixing what I found. This documentation walks through the entire process from start to finish.

---

## I. Vulnerability Assessment & Remediation (Amazon Inspector)

### 1. Lab Overview and Objectives

First, let me set the stage. The whole point of this lab was to use Amazon Inspector to scan my AWS resources - particularly Lambda functions - for vulnerabilities, and then remediate whatever issues popped up.

**My main objectives:**
- Activate Amazon Inspector
- Analyze security findings
- Remediate vulnerabilities

![1](https://github.com/user-attachments/assets/4db554a0-1fdc-441c-a912-9c5d80b2a00d)

---

### 2. Inspector Service Introduction

Before diving in, I took a moment to review what Amazon Inspector actually does. It's essentially an automated vulnerability management system that runs continuously in your AWS environment, constantly checking for security issues. Pretty powerful tool when you think about it.

![2](https://github.com/user-attachments/assets/45b5e1e9-1a40-4a6d-9597-f0263f0414ed)

---

### 3. Activating Amazon Inspector

Alright, time to actually turn this thing on. I kicked off the activation process, which grants Inspector the permissions it needs to start scanning and discovering vulnerabilities across my environment. This is where the magic begins.

![3](https://github.com/user-attachments/assets/76926d41-349e-44ba-88d2-811d379a3b8d)

---

### 4. Inspector Dashboard Summary

Once Inspector was activated, I landed on the main dashboard. At this point, it was still running its first comprehensive scan, so the findings weren't fully populated yet. But I could already see the scope of what it was covering and get a sense of the environment coverage.

![4](https://github.com/user-attachments/assets/131c9b69-dd21-4a94-81dc-d0e8cf1f1eed)

---

### 5. Reviewing Active Findings

Okay, so the scan finished and here's what Inspector found - **three Medium severity** package vulnerabilities, all sitting in my **get-request** Lambda function. Not great, but at least they weren't critical.

**The vulnerabilities it flagged:**
- CVE-2024-35195
- CVE-2024-47081
- CVE-2023-32681

![5](https://github.com/user-attachments/assets/338aa260-d18c-4ab7-a538-aad18ff4d3b8)

---

### 6. Detailed Finding Analysis (CVE-2023-32681)

I decided to dig deeper into one of these vulnerabilities to really understand what I was dealing with. I pulled up CVE-2023-32681 and got all the details - the severity level, whether a fix was available, and which package was affected. Turns out it was in the **requests library version 0.2.20.0**.

![6](https://github.com/user-attachments/assets/be82de43-c68b-4ee4-988b-f49644ae0c3a)

---

### 7. CVE-2023-32681 Description (NVD Review)

I wanted to know exactly what this vulnerability actually did, so I checked the official NVD (National Vulnerability Database) entry. The issue was specific to **Proxy-Authorization header leakage** in HTTP requests. Basically, sensitive authorization headers could potentially be leaked in certain scenarios. Good to know the details before fixing it.

![7](https://github.com/user-attachments/assets/f0d8ab5e-16e2-43af-b655-fe3fe2ba02ce)

---

### 8. Lambda Function List

Here's a view of the Lambda functions that Inspector was actively monitoring. I had two functions under continuous scanning:

**Functions being monitored:**
- **get-request**
- **generate-password-for-new-account**

Inspector was checking both of these for code and package vulnerabilities automatically.

![8](https://github.com/user-attachments/assets/48edb811-e15c-4013-bf48-674cdb21e516)

---

### 9. Lambda Code Remediation

Time to look at the actual code. I pulled up the source code for the vulnerable **get-request** function to see where this problematic library was being used. The function was pretty straightforward - it just performs a simple GET request to a sample URL. Now I knew exactly what needed fixing.

![9](https://github.com/user-attachments/assets/3c0f4eeb-32a8-4b21-a84e-85b60fc15ad2)

---

### 10. Verifying Remediation (Closed Findings)

After I updated the vulnerable library version and deployed the fix, I headed back to the Inspector dashboard to check the results. Success! All three findings successfully transitioned to **Closed** status. The vulnerabilities were officially remediated.

**Status update:**
- CVE-2024-35195: **Closed**
- CVE-2024-47081: **Closed**
- CVE-2023-32681: **Closed**

![10](https://github.com/user-attachments/assets/d132bf44-1b48-4529-908a-e31341f6ddb6)

---

### 11. Scanning Status Check

Just to make absolutely sure everything was clean, I checked the continuous scanning status for both Lambda functions. The **get-request** function now showed **No findings** - exactly what I wanted to see. Inspector was still monitoring both functions, but now they were vulnerability-free.

![11](https://github.com/user-attachments/assets/ecc47461-4bf7-4242-99f3-0ece66789c6b)

---

### 12. Inspector Lab Conclusion

And that's a wrap! I successfully completed all the objectives I set out to accomplish at the beginning of this lab.

**What I achieved:**
- Activated Amazon Inspector
- Analyzed security findings in detail
- Successfully remediated all vulnerabilities

Mission accomplished. My Lambda functions are now secure, and Inspector is continuously monitoring them for any future vulnerabilities.

![12](https://github.com/user-attachments/assets/88edb386-f78b-4bb0-982b-b9e45f3a638e)

---

## 🎯 Key Takeaways

**Amazon Inspector is powerful** - It automatically and continuously scans your AWS resources for vulnerabilities without you having to lift a finger after the initial setup.

**Vulnerability details matter** - Taking the time to understand what each CVE actually means helps you prioritize and remediate more effectively.

**The remediation loop works** - Fix the issue, deploy the update, and Inspector automatically verifies the vulnerability is gone. It's a satisfying cycle.

**Continuous monitoring is essential** - Security isn't a one-time thing. Having Inspector constantly watching for new vulnerabilities gives you peace of mind.
