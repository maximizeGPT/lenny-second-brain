# OpenClaw Bug: GPT-5.4 OAuth Not Working

> Can't get OpenClaw + GPT-5.4 + OAuth working

## Error Message

```
OAuth authentication failed
Token not received
```

## User Context

**When it happens:** Setting up GPT-5.4 with OAuth  
**Frequency:** Consistent  
**Impact:** Can't use GPT-5.4 models

## PM Analysis (5 Whys)

**Why OAuth fails?** → Token not received from OAuth provider  
**Why not received?** → Callback URL not configured  
**Why not configured?** → OAuth flow requires server callback  
**Why needs callback?** → OAuth design requires redirect  
**Root cause:** OAuth setup complex for client-only apps

## Classification

- **Type:** Auth/OAuth integration
- **Severity:** High (models blocked)
- **Category:** Authentication

## Fix Steps

### Workaround 1: Use API key instead
```json
{
  "models": {
    "gpt5": {
      "auth": "apiKey",
      "apiKey": "sk-xxx"
    }
  }
}
```

### Workaround 2: Set up local OAuth server
```bash
# Use oauth-proxy or similar
# Run locally to handle callback
```

### Workaround 3: Use different model
```json
{
  "models": {
    "default": "gpt-4o"
  }
}
```

## Prevention

**For users:**
- Use API key for testing
- Set up OAuth proxy for production
- Check OAuth provider docs

**For OpenClaw team:**
- Built-in OAuth callback handling
- Guide users through OAuth setup
- Test OAuth with each model update

## User-Friendly Explanation

> "GPT-5.4 requires a complex login (OAuth) that needs a web server to handle the response. It's like needing a mailbox to receive mail — without one, the verification gets lost. Use your API key for now or set up a simple local server to handle the OAuth flow."

---

**Added:** March 22, 2026  
**Status:** Auth complexity, workarounds available  
**Related:** [#1rocgpq](https://reddit.com/r/openclaw/comments/1rocgpq)