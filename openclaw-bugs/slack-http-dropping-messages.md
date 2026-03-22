# OpenClaw Bug: Slack HTTP Mode Dropping Messages

> Slack messages sent but never delivered

## Error Message

```
Message sent successfully
```

But message never appears in Slack.

## User Context

**When it happens:** Using Slack HTTP mode (not WebSocket)  
**Frequency:** Intermittent, silent failures  
**Impact:** Critical messages lost, users think they sent but didn't

## PM Analysis (5 Whys)

**Why message lost?** → HTTP request succeeded but Slack rejected it  
**Why rejected?** → Rate limit or invalid token scope  
**Why no error?** → HTTP 200 response, error in body  
**Why not checked?** → Code only checks status code, not response body  
**Root cause:** Silent failure pattern — success response with hidden error

## Classification

- **Type:** Silent data loss (worst kind)
- **Severity:** Critical
- **Category:** Error handling / API integration

## Fix Steps

### Detection
```bash
# Check Slack delivery logs
openclaw channels logs slack --verbose

# Look for rate limit headers
# X-RateLimit-Remaining: 0
```

### Fix: Switch to WebSocket mode
```json
// In openclaw.json
{
  "channels": {
    "slack": {
      "mode": "websocket",
      "http": {
        "enabled": false
      }
    }
  }
}
```

### Fix: Add delivery confirmation
```bash
# Verify message arrived
openclaw channels slack history --since "1 minute ago"
```

## Prevention

**For users:**
- Use WebSocket mode for critical channels
- Always verify important messages delivered
- Monitor for rate limit warnings

**For OpenClaw team:**
- Parse Slack response body for errors
- Retry with backoff on rate limits
- Confirm delivery before reporting success

## User-Friendly Explanation

> "Your message says 'sent' but Slack never got it. It's like dropping a letter in a mailbox that doesn't actually deliver. Switch to WebSocket mode for reliable delivery, or always double-check important messages actually arrived."

---

**Added:** March 22, 2026  
**Status:** Active issue, workaround available  
**Related:** [#49887](https://github.com/openclaw/openclaw/issues/49887)
