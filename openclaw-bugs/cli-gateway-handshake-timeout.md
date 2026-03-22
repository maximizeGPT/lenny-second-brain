# OpenClaw Bug: CLI Gateway Handshake Timeout

> Gateway probe fails on slow-startup hosts

## Error Message

```
gateway probe WebSocket handshake timeout
challenge timeout
```

## User Context

**When it happens:** Gateway starting on slow systems (WSL2, Docker, VPS)  
**Frequency:** Consistent on cold boot  
**Impact:** Can't use OpenClaw CLI, completely blocked

## PM Analysis (5 Whys)

**Why timeout?** → Gateway didn't respond to challenge in time  
**Why slow?** → Plugins take 8-9 seconds to initialize  
**Why plugins slow?** → Loading all integrations on startup  
**Why all at once?** → No lazy loading, everything boots together  
**Root cause:** Cold boot time exceeds challenge/response timeout

## Classification

- **Type:** Startup/timing issue
- **Severity:** High (blocks usage entirely)
- **Category:** Performance/timing

## Fix Steps

### Workaround 1: Pre-warm gateway
```bash
# Start gateway before you need it
openclaw gateway start &
sleep 15  # Wait for full boot
openclaw status
```

### Workaround 2: Disable unused plugins
```json
// In openclaw.json - disable plugins you don't use
{
  "plugins": {
    "enabled": ["core", "channels"]
    // Disable: "browser", "voice", etc.
  }
}
```

### Workaround 3: Increase timeout
```bash
# Set longer timeout for slow systems
export OPENCLAW_CHALLENGE_TIMEOUT=20000  # 20 seconds
openclaw gateway start
```

## Prevention

**For users:**
- Use `openclaw gateway start --background` and wait
- Disable plugins you don't need
- Consider faster hosting (not WSL2 for production)

**For OpenClaw team:**
- Lazy-load plugins after gateway starts
- Increase default timeout for slow systems
- Show progress during startup

## User-Friendly Explanation

> "Your computer is too slow starting OpenClaw, and it gives up before finishing. It's like trying to talk to someone who's still waking up. Either give it more time to start, or turn off features you don't use so it starts faster."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#50504](https://github.com/openclaw/openclaw/issues/50504)
