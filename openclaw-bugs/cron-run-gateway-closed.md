# OpenClaw Bug: Cron Run Causes Gateway Closed (1000)

> gateway closed (1000) error when running cron jobs

## Error Message

```
gateway closed (1000)
Connection lost during cron execution
```

## User Context

**When it happens:** Running `openclaw cron run`  
**Frequency:** Consistent  
**Impact:** Cron jobs fail, automation broken

## PM Analysis (5 Whys)

**Why closed (1000)?** → Gateway connection lost  
**Why during cron?** → Cron triggers session management issue  
**Why issue?** → Concurrent session handling bug  
**Why concurrent?** → Cron spawns new session without cleanup  
**Root cause:** Cron session lifecycle not properly managed

## Classification

- **Type:** Cron/connection issue
- **Severity:** High (automation broken)
- **Category:** Session management

## Fix Steps

### Workaround 1: Run cron with fresh session
```bash
openclaw cron run --new-session
```

### Workaround 2: Increase gateway timeout
```json
{
  "cron": {
    "timeout": 120000,
    "retryDelay": 5000
  }
}
```

### Workaround 3: Use external cron
```bash
# Run in crontab instead
*/15 * * * * curl -X POST http://localhost:3000/api/cron/execute
```

## Prevention

**For users:**
- Use --new-session flag for cron
- Monitor cron logs for connection errors
- Keep gateway running during cron windows

**For OpenClaw team:**
- Fix session lifecycle in cron
- Add proper timeout handling
- Test cron scenarios before releases

## User-Friendly Explanation

> "When your scheduled job runs, OpenClaw gets confused about which conversation to use and drops the connection. It's like someone trying to talk to you while you're on three different phone calls at once. Use a fresh session or keep your scheduled job simple."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#51632](https://github.com/openclaw/openclaw/issues/51632)