# Lenny's PM Second Brain for OpenClaw Debugger

> Apply Lenny Rachitsky's product management frameworks to debug OpenClaw like a pro PM.

## 🎯 What This Is

A Claude Project template that trains Claude on Lenny's PM frameworks, then applies them to OpenClaw debugging. Turns technical errors into product problems with systematic solutions.

## 🚀 Quick Start (Visual Guide)

### Step 1: Create Claude Project

1. Go to [claude.ai](https://claude.ai) and sign in
2. Click **"New Project"** in the left sidebar

![Step 1: Projects Menu](screenshots/step1-projects-menu.png)

3. Click the **"New Project"** button

![Step 2: New Project Button](screenshots/step2-new-project.png)

4. Name it: **"Lenny's OpenClaw Brain"**
5. Click **Create**

---

### Step 2: Add Instructions

1. In your new project, click on **"Project Instructions"**
2. Copy the contents of [`CLAUDE-PROJECT.md`](CLAUDE-PROJECT.md)
3. Paste into the instructions box
4. Click **Save**

---

### Step 3: Connect GitHub Repository

1. Click the **"+"** next to Files
2. Select **"Connect to GitHub"**

![Step 3: Add Files Menu](screenshots/step3-add-files.png)

3. Authorize Claude to access GitHub (if first time)
4. Search for: `maximizeGPT/lenny-second-brain`
5. Click **Connect**

---

### Step 4: Select Files

**Important:** Only select these folders to stay under the file limit:

✅ **Check these:**
- `lenny-curated/` (58% of capacity)
- `CLAUDE-PROJECT.md` (<1% of capacity)

❌ **Uncheck these:**
- `lenny-content/` (old version, not needed)
- `README.md` (optional)
- `example-outputs/` (optional)
- `frameworks/` (optional)

**Total should be ~59%** — well under the 100% limit.

![Step 4: Select Files](screenshots/step4-select-files.png)

Click **"Add files"** when ready.

---

### Step 5: Test It

Start a new chat and paste this test prompt:

```
I'm getting a 'gateway closed (1000)' error in OpenClaw when trying to approve devices. 
I've tried restarting but it keeps happening. Help me debug this like a PM.
```

Claude should respond with:
1. **Empathy** for your frustration
2. **Classification** of the issue type
3. **5 Whys analysis** to find root cause
4. **Clear fix steps**
5. **Prevention** suggestions

### Step 4: Test It
Try this prompt:
```
I'm getting a 'gateway closed (1000)' error in OpenClaw when trying to approve devices. 
I've tried restarting but it keeps happening. Help me debug this like a PM.
```

## 🎓 What You'll Learn

This project teaches Claude to:
- **Empathize** with frustrated users first
- **Classify** issues (technical/config/user error)
- **Apply** the 5 Whys for root cause analysis
- **Frame** fixes as user outcomes, not just technical steps
- **Prevent** future issues with systematic thinking

## 📊 Example Output

### Before (Technical Response)
> "Check your gateway config. Make sure the token is valid."

### After (PM-Style Response)
> "I hear the frustration — this error keeps blocking you. Let me apply Lenny's debugging framework:
>
> **1. User Job:** You're trying to get devices approved so you can start working
> **2. Issue Type:** Connection/auth problem (not config)
> **3. Root Cause:** The 1000 error usually means the gateway closed the WebSocket before auth completed
> **4. Fix:** [step-by-step]
> **5. Prevention:** Set up monitoring so you catch this before users do"

## 🏆 Built For Lenny's Challenge

This project was built for [Lenny's Newsletter Challenge](https://lennysnewsletter.com/p/how-i-built-lennyrpg) — applying his PM wisdom to a new domain (AI agent debugging).

**Built by:** [Mohlt](https://github.com/mohltbot)  
**Data from:** [Lenny's Newsletter](https://www.lennysnewsletter.com/)  
**Use case:** [OpenClaw Debugger](https://github.com/mohltbot/mission-control)

## 📁 Files

- `CLAUDE-PROJECT.md` — Instructions for Claude Project
- `README.md` — This file
- `example-outputs/` — Sample debugging sessions
- `frameworks/` — Extracted PM frameworks from Lenny's content

## 🤝 How to Use This

1. **For OpenClaw Users:** Use the Claude Project to debug issues with PM thinking
2. **For PMs:** Adapt this framework for your own debugging/tooling challenges
3. **For Builders:** Fork this and apply Lenny's frameworks to your domain

## 📝 License

Lenny's content is used under fair use for educational/transformative purposes. This project template is MIT licensed.

---

**Want to contribute?** Open an issue or PR with your own PM debugging frameworks!
