# Example: Debugging "Gateway Closed (1000)" Error

## User Input

> I'm getting a 'gateway closed (1000)' error in OpenClaw when trying to approve devices. I've tried restarting but it keeps happening. Help me debug this like a PM.

## Claude's PM-Style Response

---

I hear the frustration — this error keeps blocking you right when you're trying to get set up. Let me apply Lenny's debugging framework to get you unstuck.

### 1. What's the User Job-to-be-Done?

You need to approve devices so you can start using OpenClaw. The error is blocking your onboarding — this is a **retention-critical moment**.

### 2. Classify the Issue

This is a **connection/auth problem**, not a configuration issue. The "1000" code means the WebSocket closed normally, but the timing suggests it's happening *during* the auth handshake.

### 3. Apply the 5 Whys

**Why did the gateway close?** → The WebSocket connection dropped  
**Why did it drop?** → Likely auth token validation failed or timed out  
**Why did auth fail?** → Either: (a) token expired, (b) wrong scope, or (c) clock skew  
**Why didn't restart fix it?** → The token/state is cached, not cleared on restart  
**Root cause:** Stale auth state + cached invalid token

### 4. The Fix (User-Outcome Focused)

**Goal:** Get you to a working state in 5 minutes.

```bash
# Step 1: Clear the stale state (not just restart)
openclaw auth logout
openclaw auth login

# Step 2: Verify token scope
gopenclaw auth status

# Step 3: Re-approve devices with fresh session
openclaw devices list
openclaw devices approve <device-id>
```

If this fails, check:
- Is your system clock correct? (`date` should match your timezone)
- Are you on a VPN/proxy that might interfere with WebSockets?

### 5. Prevention (Product Thinking)

**For you:** Add this to your setup checklist — "Clear auth state if 1000 error persists"

**For OpenClaw team:** This is a classic "error message doesn't match root cause" problem. The fix should be:
- Detect stale auth state automatically
- Suggest "re-login" instead of generic "restart"
- Add retry logic with fresh token

---

**Did this work?** If not, let's go deeper — what's your setup (OS, OpenClaw version, network)?

## Why This Response Works

| Element | Traditional Tech Support | PM-Style (This) |
|---------|------------------------|-----------------|
| Opening | "Check your config" | Acknowledges frustration |
| Framing | Technical error code | User job-to-be-done |
| Analysis | Try random fixes | Systematic 5 Whys |
| Solution | Restart | Clear root cause (stale state) |
| Prevention | None | Checklist + product feedback |

This is how Lenny would debug a product issue — with empathy, systems thinking, and user-outcome focus.
