# OpenClaw Bug: Telegram Duplicate Messages

> Same message sent multiple times

## Error Message

```
Message sent successfully
```

But recipient receives 2-3 copies.

## User Context

**When it happens:** Sending messages via Telegram channel  
**Frequency:** Intermittent, under load  
**Impact:** Spam, user annoyance, appears unprofessional

## PM Analysis (5 Whys)

**Why duplicates?** → Message sent multiple times  
**Why multiple?** → Retry logic doesn't check delivery confirmation  
**Why no check?** → Timeout triggers retry before confirmation arrives  
**Why timeout?** → Network latency + aggressive timeout setting  
**Root cause:** Retry logic doesn't deduplicate

## Classification

- **Type:** Reliability/UX issue
- **Severity:** Medium (annoying but not blocking)
- **Category:** Message delivery

## Fix Steps

### Workaround 1: Add delay between messages
```bash
# In your script
openclaw message send --channel telegram "Hello"
sleep 2
openclaw message send --channel telegram "World"
```

### Workaround 2: Check before sending
```bash
# Get recent messages first
openclaw channels history telegram --limit 5
# Check if message already exists
```

### Workaround 3: Use message ID tracking
```json
{
  "telegram": {
    "deduplicate": true,
    "messageIdCache": 100
  }
}
```

## Prevention

**For users:**
- Add delays between rapid messages
- Implement idempotency keys
- Monitor for duplicates in logs

**For OpenClaw team:**
- Add deduplication logic
- Increase timeout before retry
- Include message ID in confirmation

## User-Friendly Explanation

> "Your message got sent twice because OpenClaw wasn't sure the first one went through, so it tried again. It's like texting someone, not seeing 'delivered', and sending again — then they get both. Add a small delay between messages or wait for confirmation."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#50450](https://github.com/openclaw/openclaw/issues/50450)
