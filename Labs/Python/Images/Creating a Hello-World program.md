# AWS Cloud9 Environment Setup & Python Workflow

Hey there! 👋 Let me walk you through how I set up and work with **AWS Cloud9** for Python development.

---

## What Am Doing Here

So basically, I want to show you how I:
- Open my AWS Cloud9 environment
- Check which Python versions I'm working with
- Avoid those annoying shell mistakes (trust me, I've made them all!)
- Actually run Python scripts without pulling my hair out 
---

## Step 1: Finding My Cloud9 Environment

**Cloud9 environments list and connection details**  

<img width="1503" height="733" alt="1" src="https://github.com/user-attachments/assets/5ec08f0d-f7da-4289-aef0-bf9f4508e539" />

So when I first log into AWS Cloud9, I land on this dashboard that shows all my environments. Here's what I see:

- **Name:** I called mine `reStart-python-cloud9` (yours might be different!)
- **Type:** It's running on an EC2 instance
- **Connection:** Uses **SSH** (Secure Shell)
- **Permission:** I'm the **Owner**

**What I do next:** I just click **Open** to launch the IDE.

**Why this matters:** It's good to confirm I actually own this environment and that it's healthy and running properly. 

---

## Step 2: Waiting for My Workspace to Load

** "Everything as you left it" loading screen**  

<img width="1854" height="917" alt="2" src="https://github.com/user-attachments/assets/e0c2ffce-ebc4-41e2-89c2-05d8b0d4072e" />

Okay, so here's something cool — when Cloud9 loads up, it actually restores my previous session! I see this loading screen with:
- A blue cloud icon with a **9** on it
- A loading bar
- The message: *"Everything as you left it."*

This is honestly one of my favorite features because all my tabs, files, and terminal sessions come back exactly how I left them. No need to reopen everything! 

---

## Step 3: My IDE Layout (Where Everything Lives)

**Cloud9 IDE welcome panel and file explorer**  

<img width="1117" height="737" alt="3" src="https://github.com/user-attachments/assets/fb5713bf-47ca-4584-b4dd-748394ef1963" />

Alright, now I'm in! Let me show you what I'm looking at:

- **File Explorer (left side):** I can see my project folder `reStart-python-clc` with some files like `.c9` and `README.md`
- **Welcome Panel (center):** Shows info about **Developer Tools**, **AWS Cloud9**, and the **Toolkit for Cloud9**
- **Terminal (bottom):** This is where I run all my commands — it shows `voclabs:~/environment $`

**Quick actions I use all the time:**
- **Create File** — for making new scripts
- **Upload Files…** — when I need to add existing files
- **Clone from GitHub** — to pull in repos

**Pro tip:** I also use the **AWS Toolkit** for working with Lambda, CloudFormation, API Gateway, and SAM. Super handy! 

---

## Step 4: Checking My Python Setup (And a Mistake I Made!)

**Terminal: working directory, Python versions, and a common error**  

<img width="1172" height="878" alt="4" src="https://github.com/user-attachments/assets/170ef6a8-387a-49c7-a4ab-2b1f5948ea18" />

Okay, so first thing I always do is verify where I am and what Python versions I have. Here's what I run:

**Finding my working directory:**
bash
pwd

→ Output: `/home/ec2-user/environment`

**Checking Python versions:**

**bash
python2
→ Python 3.8.20**

**bash
python2 --version
→ Python 2.7.18**

**bash
python3 --version
→ Python 3.8.20**

**Now here's where I messed up** (so you don't have to!):
```
I tried running this directly in bash:

print("Hello, world")


And got this error:

bash: syntax error near unexpected token "Hello, world"

**The fix:** I need to either:
1. Start the Python REPL first: `python3` → then run `print("Hello, world")`
2. Or save my code to a `.py` file and run: `python3 script.py`

**The lesson I learned:** Bash is NOT Python! I need to run Python code in the correct shell. 
```
---

## Step 5: Actually Running My Python Script

**Executing `sample.py` with arguments**  

<img width="577" height="147" alt="5" src="https://github.com/user-attachments/assets/1e2cfbfd-f303-47c7-aaec-acade2d875ab" />

Finally, let me show you how I actually run a Python script with arguments:

**Command I use:**
```bash
python3 sample.py 3 5
```

This runs my `sample.py` script and passes `3` and `5` as arguments.

---

## Wrapping Up

That's pretty much my entire workflow! Once you get the hang of these basics, working in Cloud9 becomes second nature. The key things to remember:

- Always check which shell you're in (bash vs Python)
- Use `python3` for running scripts
- Take advantage of Cloud9's session restore feature
- Keep your file explorer organized

Hope this helps you get started with Cloud9! Feel free to reach out if you get stuck on anything. 

---
