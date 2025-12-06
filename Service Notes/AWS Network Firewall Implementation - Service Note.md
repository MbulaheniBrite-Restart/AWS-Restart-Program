## 🛡️ How I Set Up AWS Network Firewall - My Project Notes

## 📋 Quick Overview

**When I Did This:** 06-12-2025 
**What I Was Doing:** Setting up AWS Network Firewall  
**The Project:** Building protection against malware threats  
**Who Did It:** Me, Brite Sendedza  
**How It Went:**  Really well!

---

## 🎯 What I Was Trying to Achieve

So basically, I wanted to set up **AWS Network Firewall** to protect my network from malware. The idea was to create a solid firewall policy that could catch and block malicious traffic before it causes problems. I used something called Suricata rules to make this happen, which was pretty interesting to learn about.

---

## 🔧 The Tools I Used (And What They Actually Do)

### AWS Network Firewall
**In Simple Terms:** Think of this as a really smart security guard for your network. It watches all the traffic coming in and going out, and it can block anything suspicious. It works at multiple levels of your network, which means it catches more threats.

**What I Did With It:** I set this up to keep an eye on everything flowing through my VPC (that's my private network space in AWS). I created rules that told it what to allow and what to block.

**The Cool Features I Got to Use:**
* Deep inspection of network traffic (it doesn't just look at the surface, it really digs in)
* Industry-standard threat detection using Suricata rules
* Smooth integration with my VPC setup

### AWS Virtual Private Cloud (VPC)
**In Simple Terms:** This is like having your own private section of the AWS cloud where you can set up your stuff exactly how you want it. It's isolated from everyone else's resources.

**What I Did With It:** I used my VPC as the place where I deployed the firewall. Everything going through certain parts of my network now gets checked by the firewall automatically.

### Suricata Rule Engine
**In Simple Terms:** Suricata is basically a powerful open-source security system that can detect and prevent intrusions. AWS Network Firewall speaks the same "language" as Suricata, which is why I used it.

**What I Did With It:** I wrote custom rules that told my firewall exactly what kind of bad stuff to look for and block. Things like connections to known malicious websites.

---

## 📝 What I Actually Did (Step by Step)

### 1. Testing Before I Started
First, I wanted to see what things looked like without any protection. So I:
* Tried connecting to a known malware site (malware.wicar.org) using a command called wget
* Yep, I could connect just fine—no protection at all
* I even downloaded some test malware files (js_crypto_miner.html and java_jre17_exec.html) just to prove the connection worked

This gave me something to compare against later.

### 2. Creating My Firewall Policy
Next, I built the foundation by creating something called LabFirewallPolicy. I set up some basic rules:
* **For normal packets:** Let them through so they can be inspected properly
* **For fragmented packets:** Drop them (these can be used by hackers to sneak past security, so I blocked them)

### 3. Making My Rule Group
This is where it got interesting. I created a StatefulRuleGroup that could hold up to 100 different security rules. I chose to use Suricata format because it's really powerful for detecting threats. This type of inspection is "stateful," which means it understands the context of the traffic, not just individual packets.

### 4. Actually Deploying the Firewall
Time to make it live! I:
* Deployed my LabFirewall into my VPC
* Connected it to my LabFirewallPolicy
* Waited for it to show as **Ready** (that was a relief!)

### 5. Connecting Everything Together
I linked my StatefulRuleGroup (with all my blocking rules) to my LabFirewallPolicy. This is what actually activated the protection.

### 6. Testing to See If It Worked
The moment of truth! I tried the same malware site connection again. And guess what? **Blocked!** The firewall shut it down immediately. I couldn't download anything malicious anymore. Success!

---

## 🚧 Where I Got Stuck

### The Suricata Rules Thing
**Here's What Happened:** So, the Suricata rule format was already there for me to use—I could download it and everything. But here's the thing: I had no idea what I was looking at. I mean, I could copy-paste the rules, but I didn't actually understand what they meant or how they worked. And I really hate just blindly copying stuff without understanding it.

**Why This Was a Problem:** I felt stuck because I couldn't confidently make changes or customize the rules. I was worried I'd break something or not set it up properly. It definitely slowed me down at first.

---

## ✅ How I Fixed It

### I Hit the Books (Well, the Internet)
**What I Did:** I decided to actually learn about Suricata instead of just winging it. I found this really helpful documentation on the official Suricata website: https://docs.suricata.io/en/latest/rules/intro.html

I spent some time reading through it, and honestly, it was worth it.

**What I Figured Out:**
* How a Suricata rule is structured (there's like an anatomy to it—action, protocol, where it's coming from, where it's going, and then options)
* How to write rules for different situations I might encounter
* The difference between telling the system to alert, drop, pass, or reject something
* How to use special keywords and modifiers to make rules more precise

**Why This Helped:** After doing this research, I felt so much more confident! I wasn't just following instructions anymore—I actually understood what I was doing and why. Now if something goes wrong, I can troubleshoot it. Plus, I can customize rules for exactly what I need.

---

## 📊 Did It Actually Work?

| What I Tested | Before I Set Up the Firewall | After I Set Up the Firewall | Did It Work? |
| :--- | :--- | :--- | :--- |
| Trying to connect to a malware site | Connected fine (uh oh) | Connection blocked (yes!) | ✅ Perfect |
| Downloading malware samples | Downloaded successfully (scary) | Access denied (phew!) | ✅ Perfect |
| Normal, legitimate traffic | Worked fine | Still works fine | ✅ Perfect |

**The Big Win:** I blocked 100% of the test connections to known bad websites, and all the normal stuff still works perfectly. That's exactly what I wanted!

---

## 🎓 What I Took Away From This

* **Understanding Beats Copying:** Taking the time to really understand Suricata rules instead of just copy-pasting made such a huge difference. Now I can actually troubleshoot problems and customize things properly.
* **Always Test First:** Creating that baseline test at the beginning was super important. It gave me a clear "before and after" so I could prove the firewall was actually doing its job.
* **Context Matters:** I learned that "stateful" inspection (which looks at the bigger picture of network traffic) is way more effective than just looking at individual packets. It's like understanding a whole conversation versus just hearing random words.

---

## 🔄 What I'd Do Next Time (Or Soon)

* **Keep the Rules Updated:** I should regularly check for new threats and update my Suricata rules to stay ahead of the bad guys
* **Add Better Monitoring:** I want to set up CloudWatch so I can see in real-time when things get blocked
* **Regular Testing:** Maybe every three months, I should run tests like this again to make sure everything's still working great
* **Plan for Growth:** I need to keep an eye on how many rules I'm using and plan ahead if I need to increase capacity

---

## 📎 Stuff I Referenced

* Suricata Rule Documentation: https://docs.suricata.io/en/latest/rules/intro.html
* AWS Network Firewall Documentation: [AWS Official Docs]
* Screenshots of Everything: [Attached separately]

---

## ✍️ Wrapping Up

**Is Everything Done?** Yes!  
**Is It More Secure Now?** Yes!  
**Did I Meet My Goals?** Yes!  

**My Final Thoughts:** This project went really well! My AWS Network Firewall is now up and running, actively protecting my VPC from malicious connections. I documented everything as I went (which helped a lot), tested thoroughly, and I'm confident it's ready for real-world use. Plus, I learned a ton about Suricata rules, which I know will come in handy for future projects.

**Finished On:** 06-12-2025  
**Done By:** Brite Sendedza
