# PM Debugging Frameworks (from Lenny's Content)

## 1. The Debugging Mindset

> "Treat bugs as product problems, not just technical issues"

**Source:** Synthesized from Lenny's podcast interviews with PMs at high-growth startups

**Core Principle:** Every error is a user experience problem. The user is trying to accomplish something, and the bug is blocking them. Your job is to:
1. Understand what they're trying to do (the job)
2. Identify why the current experience fails
3. Fix the underlying product problem, not just the symptom

**Application to OpenClaw:**
- Don't just fix the error — understand the user's workflow
- Ask: "What were they trying to do when this happened?"
- Consider: Is this blocking onboarding? Daily use? Critical workflow?

---

## 2. The 5 Whys

> "Keep asking why until you find the systemic issue"

**Source:** Classic PM framework, emphasized in Lenny's interviews

**Method:**
1. State the problem
2. Ask "Why did this happen?"
3. Ask "Why did THAT happen?"
4. Continue 5 times (or until you hit a systemic/root cause)
5. Fix the root cause, not the symptom

**Example (OpenClaw):**
- Problem: "Gateway closed (1000) error"
- Why? → WebSocket connection dropped
- Why? → Auth token invalid
- Why? → Token expired but cached
- Why? → No automatic token refresh
- Root cause: Auth system doesn't handle expiration gracefully

---

## 3. User Empathy First

> "The user is frustrated — acknowledge this first"

**Source:** Lenny's writing on PM communication

**Framework:**
1. **Acknowledge** the emotion (frustration, confusion, urgency)
2. **Validate** that this shouldn't happen
3. **Reframe** as a solvable problem
4. **Guide** to solution with confidence

**Template:**
```
"I hear the frustration — [specific pain point]. 
This [shouldn't happen / is blocking you from X].
Let me help you get unstuck."
```

---

## 4. Growth Debugging

> "Is this a scaling, onboarding, or retention issue?"

**Source:** Lenny's growth content and interviews

**Classification:**

| Issue Type | When It Happens | Example |
|------------|-----------------|---------|
| **Scaling** | High load, many users | "Works for 10 users, fails at 100" |
| **Onboarding** | First-time setup | "Can't get past device approval" |
| **Retention** | Recurring use | "Crashes every few hours" |
| **Expansion** | Power users | "Can't handle complex workflows" |

**Why It Matters:**
- Scaling issues → Infrastructure fix
- Onboarding issues → UX/docs fix
- Retention issues → Stability/reliability fix
- Expansion issues → Feature development

---

## 5. The Job-to-be-Done

> "What is the user trying to accomplish?"

**Source:** Lenny's product strategy content

**Framework:**
- Don't focus on the error — focus on the user's goal
- Ask: "If this worked perfectly, what would they do next?"
- Frame the fix in terms of enabling that outcome

**Example:**
- ❌ "Fix the WebSocket auth error"
- ✅ "Enable the user to start using OpenClaw without auth blocking onboarding"

---

## How These Frameworks Work Together

**The PM Debugging Flow:**

1. **Empathize** → Acknowledge frustration
2. **Classify** → Scaling/onboarding/retention?
3. **Identify Job** → What are they trying to do?
4. **5 Whys** → Find root cause
5. **Fix** → Solve root cause, not symptom
6. **Prevent** → How do we avoid this for next user?

This is the system encoded in the Claude Project instructions.
