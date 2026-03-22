# OpenClaw Bug: Control UI Freezes

> High CPU when switching sessions

## Error Message

```
UI unresponsive
High CPU usage
```

## User Context

**When it happens:** Switching sessions via dropdown in Control UI  
**Frequency:** With many sessions  
**Impact:** Can't use web UI, browser may crash

## PM Analysis (5 Whys)

**Why freeze?** → UI thread blocked  
**Why blocked?** → Loading all session data synchronously  
**Why all data?** → No pagination on session list  
**Why no pagination?** → Designed for small session counts  
**Root cause:** UI doesn't scale with many sessions

## Classification

- **Type:** Performance/scaling issue
- **Severity:** Medium (affects power users)
- **Category:** UI performance

## Fix Steps

### Workaround 1: Use CLI instead
```bash
# Switch sessions via command line
openclaw sessions list
openclaw sessions switch <id>
```

### Workaround 2: Archive old sessions
```bash
# Reduce session count
openclaw sessions archive --older-than 30d
```

### Workaround 3: Direct URL navigation
```
# Bypass dropdown
http://localhost:3000/session/<id>
```

## Prevention

**For users:**
- Archive sessions regularly
- Use CLI for session management
- Don't keep hundreds of active sessions

**For OpenClaw team:**
- Paginate session dropdown
- Lazy-load session data
- Virtual scrolling for large lists

## User-Friendly Explanation

> "Your OpenClaw web interface is trying to show every conversation you've ever had at once, and it's overwhelming your browser. It's like trying to open every file on your computer at the same time. Use the command line instead, or clean up old conversations."

---

**Added:** March 22, 2026  
**Status:** Performance issue, workarounds available  
**Related:** [#51685](https://github.com/openclaw/openclaw/issues/51685)
