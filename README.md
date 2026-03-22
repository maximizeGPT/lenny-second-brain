# Lenny's PM Second Brain for OpenClaw Debugger

> Apply Lenny Rachitsky's product management frameworks to debug OpenClaw like a pro PM.

**Built for [Lenny's Newsletter Challenge](https://lennysnewsletter.com/p/how-i-built-lennyrpg)** — applying PM wisdom to AI agent debugging.

---

## 🎯 What This Is

A Claude Project template that trains Claude on Lenny's PM frameworks, then applies them to OpenClaw debugging. Turns technical errors into product problems with systematic solutions.

**Perfect for:**
- OpenClaw users frustrated with cryptic errors
- PMs who want to debug with empathy and systems thinking
- Anyone building AI tools who needs better debugging workflows

---

## 🚀 Quick Start (5 Minutes)

### Step 1: Create Claude Project

1. Go to [claude.ai](https://claude.ai) and sign in
2. Click **"New Project"** in the left sidebar
3. Click **"New Project"** button
4. Name it: **"Lenny's OpenClaw Brain"**
5. Click **Create**

### Step 2: Add Instructions

1. In your new project, click on **"Project Instructions"**
2. Copy the contents of [`CLAUDE-PROJECT.md`](CLAUDE-PROJECT.md)
3. Paste into the instructions box
4. Click **Save**

### Step 3: Connect This Repository

1. Click the **"+"** next to Files
2. Select **"Connect to GitHub"**
3. Authorize Claude to access GitHub (if first time)
4. Search for: `maximizeGPT/lenny-second-brain`
5. Click **Connect**

### Step 4: Select Folders

**Connect these two folders:**
- ✅ `lenny-curated/` — Lenny's best PM content
- ✅ `openclaw-bugs/` — Real OpenClaw debugging cases

**Total: ~60% of file capacity** — well under the limit.

Click **"Add files"** when ready.

### Step 5: Test It

Start a new chat and try:

```
I'm getting a 'gateway closed (1000)' error in OpenClaw when trying to approve devices. 
I've tried restarting but it keeps happening. Help me debug this like a PM.
```

Claude should respond with empathy, classification, 5 Whys analysis, clear fix steps, and prevention suggestions.

---

## 📁 Repository Structure

```
lenny-second-brain/
├── lenny-curated/          # Lenny's best PM content (748KB)
│   ├── how-to-build-your-pm-second-brain-with-chatgpt.md
│   ├── everyone-should-be-using-claude-code-more.md
│   ├── product-manager-is-an-unfair-role-so-work-unfairly.md
│   └── ... (10 newsletters + 5 podcasts)
├── openclaw-bugs/          # Real debugging cases (updated regularly)
│   ├── gateway-closed-1000.md
│   ├── whatsapp-no-listener.md
│   └── ... (add new bugs here!)
├── CLAUDE-PROJECT.md       # Instructions for Claude
└── README.md              # This file
```

---

## 🎓 What You'll Learn

This project teaches Claude to:
- **Empathize** with frustrated users first
- **Classify** issues (scaling/onboarding/retention)
- **Apply** the 5 Whys for root cause analysis
- **Frame** fixes as user outcomes, not just technical steps
- **Prevent** future issues with systematic thinking

---

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

---

## 🏆 Built For Lenny's Challenge

This project was built for [Lenny's Newsletter Challenge](https://lennysnewsletter.com/p/how-i-built-lennyrpg) — applying his PM wisdom to a new domain (AI agent debugging).

**Built by:** [Mohlt](https://github.com/mohltbot)  
**Data from:** [Lenny's Newsletter](https://www.lennysnewsletter.com/)  
**Use case:** [OpenClaw Debugger](https://github.com/mohltbot/mission-control)

---

## 🤝 Contributing

**Found a new OpenClaw bug?** Add it to `openclaw-bugs/` folder with:
- Error message
- Root cause analysis
- Fix steps
- Prevention tips

**Want to improve the framework?** Open a PR with your PM debugging techniques!

---

## 📝 License

Lenny's content is used under fair use for educational/transformative purposes. This project template is MIT licensed.

---

**Star this repo** ⭐ if it helps you debug OpenClaw like a PM!
