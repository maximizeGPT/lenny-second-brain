# OpenClaw Bug: Browser MCP Auto-Read Token Failure

> --browser-profile user doesn't read gateway.remote.token

## Error Message

```
Browser authentication required
Token not found
```

## User Context

**When it happens:** Using Chrome MCP with user profile  
**Frequency:** Consistent  
**Impact:** Can't use browser automation without manual login

## PM Analysis (5 Whys)

**Why no token?** → Profile doesn't inherit gateway credentials  
**Why not inherited?** → Browser profile runs in separate context  
**Why separate?** → Security isolation per profile  
**Why isolation?** → Browser profile design  
**Root cause:** Token circulation between gateway and browser profile not implemented

## Classification

- **Type:** Auth/automation issue
- **Severity:** High (blocks browser automation)
- **Category:** Integration/auth

## Fix Steps

### Workaround 1: Manual token export
```bash
# Get token first
openclaw gateway token
# Set as env var
export OPENCLAW_TOKEN="your-token"
openclaw browser --profile user
```

### Workaround 2: Use default profile
```bash
# Don't use --browser-profile
openclaw browser
```

### Workaround 3: Use cookie import
```bash
# Export cookies from logged-in browser
openclaw browser cookies export
# Import to user profile
openclaw browser cookies import
```

## Prevention

**For users:**
- Use default browser profile for automation
- Export/import cookies for persistent auth
- Set token manually for user profiles

**For OpenClaw team:**
- Auto-inherit token in user profiles
- Better error messages for auth failures
- Support token passthrough

## User-Friendly Explanation

> "Your browser profile runs in its own secure box that doesn't share your login with OpenClaw. It's like being logged into two different browsers — you'd need to log in again in the second one. Either use the default profile or copy your login cookies over."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#51661](https://github.com/openclaw/openclaw/issues/51661)