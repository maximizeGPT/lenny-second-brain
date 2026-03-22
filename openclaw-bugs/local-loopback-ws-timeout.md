# OpenClaw Bug: Local Loopback Gateway WS Handshake Timeout

> WebSocket handshake times out on local connection

## Error Message

```
WebSocket handshake timeout
Local connection failed
```

## User Context

**When it happens:** Running OpenClaw locally with local loopback  
**Frequency:** 2026.3.13 regression  
**Impact:** Can't use local MCP commands

## PM Analysis (5 Whys)

**Why timeout?** → Handshake not completing  
**Why not completing?** → Localhost routing different path  
**Why different path?** → 2026.3.13 changed local connection logic  
**Why changed?** → Security update for local connections  
**Root cause:** Local loopback path not properly handled in 2026.3.13

## Classification

- **Type:** Regression/connectivity issue
- **Severity:** High (blocks local CLI)
- **Category:** Network/connection

## Fix Steps

### Workaround 1: Use explicit IP
```bash
# Instead of localhost, use 127.0.0.1
openclaw gateway start --host 127.0.0.1
```

### Workaround 2: Disable local checks
```json
{
  "network": {
    "localLoopback": {
      "enabled": true,
      "timeout": 30000
    }
  }
}
```

### Workaround 3: Use HTTP instead of WS
```bash
openclaw gateway start --http-only
```

## Prevention

**For users:**
- Use 127.0.0.1 instead of localhost
- Increase timeout if on slow machine
- Watch release notes for regressions

**For OpenClaw team:**
- Test local loopback before releases
- Preserve local connection path
- Better timeout handling

## User-Friendly Explanation

> "On 2026.3.13, OpenClaw's local connection got stricter and won't talk to itself on localhost. It's like you moved to a new house and didn't update your address book. Use 127.0.0.1 instead of localhost, or check for updates that fix this."

---

**Added:** March 22, 2026  
**Status:** 2026.3.13 regression, workarounds available  
**Related:** [#51679](https://github.com/openclaw/openclaw/issues/51679)