# Service Note: Amazon Inspector Vulnerability Assessment & Remediation

**Date:** 03-12-2025  
**Created by** Brite Sendedza  
**Project:** AWS Lambda Security Scanning with Amazon Inspector  
**Service:** Amazon Inspector  
**Status:** Completed Successfully

---

## Overview

I needed to set up continuous vulnerability scanning for my AWS Lambda functions, so I activated Amazon Inspector and used it to identify, analyze, and remediate security vulnerabilities. This service note documents the entire workflow from activation through successful remediation.

---

## What I Accomplished

**Service Activated:** Amazon Inspector (continuous scanning)  
**Resources Scanned:** 2 AWS Lambda functions  
**Initial Findings:** 3 Medium severity vulnerabilities  
**Vulnerabilities Remediated:** 3/3 (100% resolution)  
**Final Status:** Zero active findings, continuous monitoring enabled

---

## AWS Tools & Services I Used

### 1. **Amazon Inspector**

Inspector is AWS's automated vulnerability management service, and honestly, it's pretty impressive. Once you activate it, it continuously scans your AWS resources - EC2 instances, container images, Lambda functions - looking for software vulnerabilities and network configuration issues. The cool thing is it doesn't just tell you about problems, it also tells you how severe they are and whether fixes are available.

What makes Inspector really valuable is that it's continuous. It's not a one-time scan that gets outdated the moment you deploy new code. It keeps monitoring your environment 24/7 and alerts you when new vulnerabilities are discovered or when you deploy code with known issues. For my project, I focused on Lambda function scanning, which checks both the code and the package dependencies for security issues.

### 2. **AWS Lambda**

Lambda is AWS's serverless compute service - basically, you write code and AWS handles all the server management, scaling, and infrastructure. I had two Lambda functions in my environment that Inspector was monitoring:

- **get-request** - A simple function that makes HTTP requests
- **generate-password-for-new-account** - Does exactly what the name suggests

The beauty of Lambda is you don't have to worry about servers, but the challenge is making sure your code and dependencies are secure. That's where Inspector comes in handy.

### 3. **National Vulnerability Database (NVD)**

While not technically an AWS service, the NVD is what Inspector references for vulnerability information. Every CVE (Common Vulnerabilities and Exposures) that Inspector finds is documented in the NVD with detailed descriptions, severity scores, and remediation guidance. I used the NVD to really understand what each vulnerability meant before fixing it.

---

## Functions Monitored

**Function 1: get-request**
- Purpose: Makes HTTP GET requests to external URLs
- Initial Status: 3 Medium severity vulnerabilities found
- Vulnerable Package: requests library version 0.2.20.0
- Final Status: All vulnerabilities remediated, no findings

**Function 2: generate-password-for-new-account**
- Purpose: Generates secure passwords for new account creation
- Status: No vulnerabilities detected
- Monitoring: Continuous scanning active

---

## Vulnerabilities Identified

### CVE-2024-35195
- Severity: Medium
- Package: requests library
- Status: Closed (Remediated)

### CVE-2024-47081
- Severity: Medium
- Package: requests library
- Status: Closed (Remediated)

### CVE-2023-32681
- Severity: Medium
- Issue: Proxy-Authorization header leakage in HTTP requests
- Package: requests library version 0.2.20.0
- Description: This vulnerability could potentially leak sensitive authorization headers in certain proxy scenarios
- Status: Closed (Remediated)

---

## What I Actually Did

### Phase 1: Inspector Activation

First, I navigated to the Amazon Inspector console and kicked off the activation process. This grants Inspector the necessary permissions to scan resources across my AWS environment. The activation was straightforward - just a few clicks and it was up and running.

### Phase 2: Initial Scan & Analysis

Once Inspector was active, it immediately began scanning my Lambda functions. Within minutes, the dashboard populated with findings. I had three Medium severity vulnerabilities, all in the get-request function, all related to the requests library.

I didn't just look at the high-level findings though. I clicked into each vulnerability to understand exactly what the issue was. For CVE-2023-32681, I even went to the official NVD page to read the full technical description. Understanding the actual risk helped me prioritize the remediation work.

### Phase 3: Code Review

I pulled up the source code for the get-request function to see how the vulnerable library was being used. The function was pretty simple - it just makes HTTP GET requests - but it was using an outdated version of the requests library that had known security issues.

### Phase 4: Remediation

Time to fix things. I updated the requests library to the latest secure version in my Lambda function's dependencies. After updating, I redeployed the function with the patched version.

### Phase 5: Verification

After deploying the fix, I went back to the Inspector dashboard to confirm the remediation worked. Sure enough, all three findings transitioned to Closed status. The continuous scanning confirmed that the get-request function now showed No findings. Success!

---

## Challenges

### Understanding Vulnerability Severity and Priority
- Inspector flagged three Medium severity issues, but I wasn't sure which one to tackle first
- CVE descriptions were technical and required research to understand the actual risk
- Had to determine if the vulnerabilities were actually exploitable in my specific use case

### Dependency Management in Lambda
- Lambda functions bundle their dependencies, making version updates less straightforward than regular applications
- Needed to ensure the updated library version was compatible with my existing code
- Had to redeploy the entire function to apply the dependency update

### Verifying Remediation Success
- Wasn't immediately clear how long it would take for Inspector to re-scan and close the findings
- Needed confirmation that the fix actually resolved the issue, not just masked it

---

## Solutions

### Prioritizing with NVD Research
- I used the National Vulnerability Database to read detailed descriptions of each CVE
- Focused on the vulnerability with the most specific exploit scenario (CVE-2023-32681)
- Realized all three were in the same package, so fixing one would likely fix all

### Systematic Dependency Update Process
- Checked the requests library documentation for the latest stable version
- Updated the requirements file with the secure version
- Tested the function locally before deploying to ensure compatibility
- Redeployed with confidence knowing the code still functioned correctly

### Trusting the Automated Verification
- Inspector automatically re-scans after changes are detected
- Within minutes of redeployment, findings were marked as Closed
- Continuous monitoring means any new vulnerabilities will be caught immediately
- The No findings status gave me confidence the remediation was successful

---

## Verification & Testing

**Inspector Activation:** Service activated successfully with proper permissions  
**Initial Scan:** Completed within minutes, identified 3 vulnerabilities  
**Vulnerability Analysis:** Reviewed each CVE in detail via NVD  
**Code Review:** Identified vulnerable library usage in get-request function  
**Remediation:** Updated requests library to secure version  
**Redeployment:** Function redeployed with patched dependencies  
**Verification:** All 3 findings transitioned to Closed status  
**Continuous Monitoring:** Both functions now under ongoing surveillance

---

## Key Takeaways

**Inspector is set-it-and-forget-it security** - Once activated, it continuously monitors without requiring constant attention. It just works in the background.

**Medium severity vulnerabilities still matter** - Even if they're not critical, they represent real security risks that should be addressed promptly.

**The NVD is your friend** - Taking time to understand the actual vulnerability through the NVD helps you make informed decisions about priority and remediation approach.

**Lambda dependency updates require redeployment** - Unlike traditional applications, you can't just update a library in place. The entire function needs to be repackaged and redeployed.

**Automation validates your work** - Inspector automatically verifying your fixes saves time and gives you confidence the remediation was successful.

**Continuous monitoring provides peace of mind** - Knowing that Inspector is always watching for new vulnerabilities means you're not flying blind between manual security audits.

---

## Best Practices Applied

**Activated Inspector early** - The sooner you turn it on, the sooner you know about vulnerabilities. Don't wait until you have a security incident.

**Investigated before remediating** - Understanding what each CVE actually meant helped me fix the root cause, not just apply patches blindly.

**Updated to latest stable versions** - Instead of just patching the specific vulnerability, I updated to the current stable release to get all security fixes.

**Verified through multiple checks** - Checked both the Inspector dashboard and the function's scanning status to confirm remediation.

**Maintained continuous monitoring** - Left Inspector active to catch any future vulnerabilities automatically.

---

## Next Steps

Now that I've got Inspector working and my Lambda functions secured, here's what I'm planning next:

- Expand Inspector coverage to scan EC2 instances and container images
- Set up SNS notifications for new high/critical findings so I get alerted immediately
- Establish a regular cadence for reviewing and addressing Medium/Low findings
- Document the remediation process as a runbook for the team
- Consider integrating Inspector findings into our CI/CD pipeline to catch vulnerabilities before deployment
- Review other Lambda functions in the environment to ensure they're all being monitored

---

## Resources & Configuration

**Service:** Amazon Inspector  
**Scan Type:** Continuous (automated)  
**Resource Coverage:** AWS Lambda functions  
**Functions Monitored:** 2  
**Initial Findings:** 3 Medium severity  
**Remediated:** 3/3 (100%)  
**Current Status:** Zero active findings

**Vulnerable Package:** requests library 0.2.20.0  
**Remediation Action:** Updated to latest secure version  
**Verification Method:** Inspector automatic re-scan

---

**Final Thoughts:** Setting up Amazon Inspector was one of those things I wish I'd done sooner. The continuous monitoring takes security from something you check occasionally to something that's always being watched. The vulnerability remediation process was straightforward once I understood what each CVE meant, and seeing those findings transition to Closed status was genuinely satisfying. For anyone running Lambda functions (or really any AWS resources), Inspector is absolutely worth activating. The peace of mind alone makes it valuable, but catching vulnerabilities before they become incidents? That's priceless.
