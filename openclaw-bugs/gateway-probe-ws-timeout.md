# OpenClaw Bug: Gateway Probe Timeout (HTTP Works)

> WebSocket gateway probe fails, HTTP works fine

## Error Message

```
WebSocket connection failed
HTTP probe works fine
```

## User Context

**When it happens:** Testing gateway connectivity  
**Frequency:** Does not affect functionality  
**Impact:** Monitoring tools fail, false alarms

## PM Analysis (5 Whys)

**Why WS fail?** → Network path difference for WS vs HTTP  
**Why difference?** → Firewall or proxy only blocks WS  
**Why only WS?** → HTTP uses port 80/443, WS might use different port  
**Why different?** → Gateway configuration  
**Root cause:** Network-level WS blocking

## Classification

- **Type:** Network/connectivity issue
- **Severity:** Low (functionality works)
- **Category:** Monitoring

## Fix Steps

### Workaround 1: Update monitoring to use HTTP
```bash
# Check with HTTP instead
curl http://localhost:3000/health
```

### Workaround 2: Configure firewall
```bash
# Allow WS port
ufw allow 3001/tcp
```

### Workaround 3: Use full gateway URL
```bash
# Include ws:// in URL
openclaw gateway status --url ws://localhost:3001
```

## Prevention

**For users:**
- Test with HTTP if WS fails
- Check firewall rules
- Use appropriate monitoring method

**For OpenClaw team:**
- Unified health endpoint for WS/HTTP
- Better diagnostic messages
- WS fallback to HTTP

## User-Friendly Explanation

> "Your network is set up to let regular web traffic through but blocks the realtime WebSocket connection. It's like having a door for walking but not for running — both are you, but only one gets through. Use HTTP for monitoring or ask your network administrator to open the WebSocket port."

---

**Added:** March 22, 2026  
**Status:** Network issue, workarounds available  
**Related:** [#51698](https://github.com/openclaw/openclaw/issues/51698)