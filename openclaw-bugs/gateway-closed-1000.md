# OpenClaw Bug: Gateway Closed (1000)

> WebSocket connection drops during device approval

## Error Message

```
gateway closed (1000)
```

## User Context

**When it happens:** During device approval/setup  
**Frequency:** Intermittent, often after restarts  
**Impact:** Blocks onboarding — user can't start using OpenClaw

## PM Analysis (5 Whys)

**Why did the gateway close?** → WebSocket connection dropped  
**Why did it drop?** → Auth token validation failed or timed out  
**Why did auth fail?** → Token expired but cached in browser/app  
**Why didn't restart fix it?** → Cache not cleared, stale state persists  
**Root cause:** Auth state management doesn't handle expiration gracefully

## Classification

- **Type:** Onboarding blocker
- **Severity:** High (prevents new users from starting)
- **Category:** Auth/connection issue

## Fix Steps

```bash
# Step 1: Clear stale auth state (not just restart)
openclaw auth logout
openclaw auth login

# Step 2: Verify fresh token
gopenclaw auth status

# Step 3: Retry device approval
openclaw devices list
openclaw devices approve <device-id>
```

**If still failing:**
- Check system clock: `date` (must match timezone)
- Check VPN/proxy: Disable temporarily
- Try incognito/private browser window

## Prevention

**For users:**
- Add to onboarding checklist: "If 1000 error, re-login before retrying"
- Document: "Restart ≠ re-login"

**For OpenClaw team:**
- Auto-detect stale auth and suggest re-login
- Clearer error message: "Session expired, please log in again"
- Auto-retry with fresh token before showing error

## User-Friendly Explanation

> "This error means OpenClaw lost connection to the server during setup. It's usually because your login session expired. The fix is simple: log out and back in (don't just restart). This gives you a fresh session and should resolve it immediately."

---

**Added:** March 22, 2026  
**Status:** Active issue, workaround documented  
**Related:** [#47103](https://github.com/openclaw/openclaw/issues/47103), [#45504](https://github.com/openclaw/openclaw/issues/45504)
