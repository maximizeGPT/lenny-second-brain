# OpenClaw Bug: Discord WebSocket Disconnects

> Discord bot disconnects every ~10 minutes

## Error Message

```
Discord WebSocket closed
Reconnecting...
```

## User Context

**When it happens:** Discord bot running in production  
**Frequency:** Every 10 minutes (health-monitor restart loop)  
**Impact:** Messages lost during disconnects, unreliable bot

## PM Analysis (5 Whys)

**Why disconnect?** → WebSocket connection closed  
**Why closed?** → Health monitor thinks bot is unhealthy  
**Why unhealthy?** → Heartbeat timing issue or missed acks  
**Why timing issue?** → Network latency + aggressive timeout settings  
**Root cause:** Discord gateway timeout too aggressive for real-world networks

## Classification

- **Type:** Reliability/connection issue
- **Severity:** High (production bots affected)
- **Category:** Network/timeout configuration

## Fix Steps

### Workaround 1: Increase heartbeat interval
```json
// In openclaw.json
{
  "channels": {
    "discord": {
      "heartbeatInterval": 30000,
      "reconnectDelay": 5000
    }
  }
}
```

### Workaround 2: Disable hot reload
```bash
# SIGUSR1 hot reload causes gateway death
# Don't use: kill -USR1 <pid>
# Use full restart instead: openclaw gateway restart
```

### Workaround 3: External health check
```bash
# Use external monitoring instead of built-in health monitor
# Disable: openclaw config set channels.discord.healthMonitor false
```

## Prevention

**For users:**
- Monitor for disconnect patterns (every 10 min = health monitor)
- Use external health checks (UptimeRobot, Pingdom)
- Log all Discord events to detect message loss

**For OpenClaw team:**
- Increase default Discord timeout
- Separate health check from WebSocket connection
- Add queue for messages during reconnection

## User-Friendly Explanation

> "Your Discord bot keeps getting 'health checked' to death. The system thinks it's unhealthy every 10 minutes and restarts it, causing disconnects. You need to either make the health check less aggressive or disable it and use external monitoring instead."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#51116](https://github.com/openclaw/openclaw/issues/51116)
