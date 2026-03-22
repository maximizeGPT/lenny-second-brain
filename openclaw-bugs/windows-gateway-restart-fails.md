# OpenClaw Bug: Windows Gateway Restart Fails

> schtasks error on Windows restart

## Error Message

```
schtasks: Task already exists
Gateway restart failed
```

## User Context

**When it happens:** Restarting gateway on Windows  
**Frequency:** Every restart attempt  
**Impact:** Can't restart gateway without manual intervention

## PM Analysis (5 Whys)

**Why fail?** → Scheduled task already exists  
**Why exists?** → Previous restart didn't clean up task  
**Why not cleaned?** → Task creation fails before cleanup runs  
**Why fail first?** → Race condition on restart  
**Root cause:** Task management logic doesn't handle existing tasks

## Classification

- **Type:** Platform-specific issue
- **Severity:** High (Windows users blocked)
- **Category:** Process management

## Fix Steps

### Fix 1: Delete task manually
```powershell
# Run as Administrator
schtasks /delete /tn "OpenClawGateway" /f
openclaw gateway restart
```

### Fix 2: Use manual restart
```powershell
# Stop and start instead of restart
openclaw gateway stop
Start-Sleep -s 5
openclaw gateway start
```

### Fix 3: Disable Windows service integration
```json
{
  "gateway": {
    "windows": {
      "useService": false
    }
  }
}
```

## Prevention

**For users:**
- Use manual stop/start instead of restart
- Run as Administrator
- Check Task Scheduler for stuck tasks

**For OpenClaw team:**
- Delete existing task before creating new one
- Better error handling for schtasks
- Support non-service mode on Windows

## User-Friendly Explanation

> "Windows thinks OpenClaw is already running a restart task and won't let you start another one. It's like trying to start your car when it's already trying to start. You need to manually delete the stuck task first, or just stop and start OpenClaw normally instead of using restart."

---

**Added:** March 22, 2026  
**Status:** Windows-specific, workaround available  
**Related:** [#49871](https://github.com/openclaw/openclaw/issues/49871)
