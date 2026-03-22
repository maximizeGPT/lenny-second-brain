# OpenClaw Bug: Trashed Session Messages Re-delivered

> Deleted session's messages appear again on restart

## Error Message

```
[Old message from trashed session]
```

Messages from deleted sessions reappear.

## User Context

**When it happens:** After deleting session and restarting gateway  
**Frequency:** Every restart  
**Impact:** Data pollution, confusion, privacy issue

## PM Analysis (5 Whys)

**Why old messages?** → Gateway re-processed old queue  
**Why still in queue?** → Outbound message queue not cleared on trash  
**Why not cleared?** → Trash action only marks deleted, doesn't purge queue  
**Why only mark?** → Soft delete pattern for recovery  
**Root cause:** Trash doesn't clean up message queue

## Classification

- **Type:** Data integrity/privacy issue
- **Severity:** High (data resurrection)
- **Category:** State management

## Fix Steps

### Immediate Fix: Purge queue manually
```bash
# Stop gateway
openclaw gateway stop

# Clear outbound queue
rm -rf ~/.openclaw/outbound-queue/*

# Restart
openclaw gateway start
```

### Ongoing Fix: Hard delete
```bash
# Use --hard flag when trashing
openclaw sessions trash <id> --hard

# Or purge after trash
openclaw sessions purge --trashed
```

## Prevention

**For users:**
- Use `--hard` flag when deleting sensitive sessions
- Purge trashed sessions regularly
- Check for zombie messages after restart

**For OpenClaw team:**
- Trash should clear message queue
- Add confirmation: "Also delete 50 pending messages?"
- Don't re-process trashed session queues

## User-Friendly Explanation

> "You deleted a conversation but OpenClaw kept a copy of the unsent messages. When you restarted, it tried to send them again. It's like throwing away a letter but the post office still delivers it. Use 'hard delete' to actually destroy the messages."

---

**Added:** March 22, 2026  
**Status:** Active issue, workaround available  
**Related:** [#50496](https://github.com/openclaw/openclaw/issues/50496)
