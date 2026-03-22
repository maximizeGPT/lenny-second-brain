# OpenClaw Bug: Telegram Responses Not Sent

> Responses missing "run done" event

## Error Message

```
Run completed
```

But no response sent to Telegram.

## User Context

**When it happens:** Telegram channel running automation  
**Frequency:** Intermittent  
**Impact:** Automation produces no output to user

## PM Analysis (5 Whys)

**Why no response?** → "run done" event not triggering  
**Why not triggering?** → Event listener disconnected  
**Why disconnected?** → WebSocket dropped but not reconnected  
**Why dropped?** → Network issue or server restart  
**Root cause:** Event system doesn't auto-reconnect listeners

## Classification

- **Type:** Event/delivery issue
- **Severity:** High (silent failures)
- **Category:** Message delivery

## Fix Steps

### Workaround 1: Restart Telegram channel
```bash
# Stop and restart
openclaw channels stop telegram
openclaw channels start telegram
```

### Workaround 2: Use polling mode
```json
{
  "channels": {
    "telegram": {
      "mode": "polling"
    }
  }
}
```

### Workaround 3: Add health check
```bash
# Check channel status
openclaw channels status telegram
```

## Prevention

**For users:**
- Monitor channel status regularly
- Use polling mode for reliability
- Set up alerts for channel failures

**For OpenClaw team:**
- Auto-reconnect event listeners
- Detect and alert on silent failures
- Add channel health monitoring

## User-Friendly Explanation

> "Your Telegram channel disconnected its event listener and now misses the signal that says 'I'm done.' It's like your phone on silent — it's working but you're not getting the notifications. Restart the channel or switch to polling mode to make sure you get messages."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#51659](https://github.com/openclaw/openclaw/issues/51659)