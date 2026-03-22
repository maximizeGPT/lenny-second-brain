# OpenClaw Bug: Memory Leak (sessions.json)

> Gateway memory grows unbounded, sessions.json huge

## Error Message

```
Gateway memory usage: 1.3GB+
sessions.json: 500MB+
```

## User Context

**When it happens:** Long-running gateway, many sessions  
**Frequency:** Gradual over days/weeks  
**Impact:** System slowdown, eventual crash, data corruption

## PM Analysis (5 Whys)

**Why huge memory?** → sessions.json keeps growing  
**Why growing?** → Old sessions not cleaned up  
**Why not cleaned?** → No TTL or pruning mechanism  
**Why no pruning?** → Design assumes sessions are long-lived  
**Root cause:** Session storage designed for retention, not rotation

## Classification

- **Type:** Scaling/resource issue
- **Severity:** High (affects long-running deployments)
- **Category:** Memory management

## Fix Steps

### Immediate Fix: Manual cleanup
```bash
# Stop gateway
openclaw gateway stop

# Backup and truncate sessions
mv ~/.openclaw/sessions.json ~/.openclaw/sessions.json.backup
echo "[]" > ~/.openclaw/sessions.json

# Restart
openclaw gateway start
```

### Ongoing Fix: Cron cleanup
```bash
# Add to crontab - weekly cleanup
0 0 * * 0 openclaw sessions purge --older-than 30d
```

### Config Fix: Set limits
```json
// In openclaw.json
{
  "sessions": {
    "maxAge": "30d",
    "maxCount": 1000,
    "autoPurge": true
  }
}
```

## Prevention

**For users:**
- Monitor `sessions.json` size: `ls -lh ~/.openclaw/sessions.json`
- Set up weekly purge cron job
- Alert if gateway memory > 500MB

**For OpenClaw team:**
- Auto-purge sessions older than X days
- Compress old session data
- Add memory usage warnings

## User-Friendly Explanation

> "OpenClaw remembers every session forever, like a hoarder. Eventually your disk fills up and everything slows down. You need to periodically clean out old sessions — think of it as spring cleaning for your AI."

---

**Added:** March 22, 2026  
**Status:** Active issue, workaround available  
**Related:** [#51097](https://github.com/openclaw/openclaw/issues/51097)
