# OpenClaw Bug: Subagent Spawn Fails

> Subagent starts but produces no output

## Error Message

```
Subagent failed to execute or produce output
```

## User Context

**When it happens:** Running subagent tasks  
**Frequency:** Intermittent, subagent goes "stale" over time  
**Impact:** Can't offload work to subagents, blocks automation

## PM Analysis (5 Whys)

**Why no output?** → Subagent process didn't complete  
**Why didn't it complete?** → Session state corrupted  
**Why corrupted?** → Previous subagent didn't clean up properly  
**Why no cleanup?** → Subagent runtime doesn't isolate sessions  
**Root cause:** Session isolation bug in subagent spawn

## Classification

- **Type:** Scaling/reliability issue
- **Severity:** Medium (affects power users)
- **Category:** Runtime/environment issue

## Fix Steps

```bash
# Step 1: Clear subagent state
openclaw sessions purge

# Step 2: Check concurrent limit
openclaw config get agents.defaults.subagents.maxConcurrent

# Step 3: Add delay between spawns
# In your script:
sleep 2
openclaw agent spawn --task "your task"
```

**For WSL2 users:**
```bash
# Check PATH issues
PATH=/usr/bin:/bin openclaw gateway start

# Check permissions
chmod +x agent.sh
```

## Prevention

**For users:**
- Purge sessions before batch subagent runs
- Add retry logic with exponential backoff
- Monitor subagent success rate

**For OpenClaw team:**
- Auto-cleanup subagent sessions
- Better error messages: "Subagent timeout, try purging sessions"
- Session isolation per subagent spawn

## User-Friendly Explanation

> "Subagents are like temporary workers — sometimes they don't clean up after themselves, and that messes up the next worker. The fix is to clear the workspace (purge sessions) before starting new subagents. If you're running many in a row, add a small delay between them."

---

**Added:** March 22, 2026  
**Status:** Known issue, workaround documented  
**Related:** [#51062](https://github.com/openclaw/openclaw/issues/51062)
