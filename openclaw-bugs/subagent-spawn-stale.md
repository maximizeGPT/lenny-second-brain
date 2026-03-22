# OpenClaw Bug: Subagent Spawn Issues

> Subagent goes stale, new sessions don't help

## Error Message

```
Subagent stale
No response from subagent
```

## User Context

**When it happens:** Spawning subagents for background tasks  
**Frequency:** Intermittent  
**Impact:** Background automation fails

## PM Analysis (5 Whys)

**Why stale?** → Subagent process stuck  
**Why stuck?** → Session state not properly initialized  
**Why not initialized?** → Spawn logic missing cleanup  
**Why missing?** → Race condition in spawning  
**Root cause:** Subagent lifecycle not fully managed

## Fix Steps

### Workaround 1: Kill stale subagents
```bash
openclaw subagents kill --all
openclaw subagents spawn <task>
```

### Workaround 2: Use sync mode
```bash
openclaw subagents spawn <task> --sync
```

### Workaround 3: Restart gateway
```bash
openclaw gateway restart
```

## Prevention

**For users:**
- Monitor subagent status
- Kill stale subagents before spawning new
- Use sync mode for critical tasks

**For OpenClaw team:**
- Auto-cleanup stale subagents
- Better lifecycle management
- Health checks for subagents

## User-Friendly Explanation

> "Your background worker got stuck and won't respond. It's like a jobber who started working then went AFK. Kill them and start fresh, or use sync mode where you wait for the work to finish before moving on."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** Reddit thread (Rich_Chef_6141)