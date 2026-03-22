# OpenClaw Bug: WhatsApp Message React Fails

> WhatsApp message reactions fail after send

## Error Message

```
No active WhatsApp Web listener
React failed
```

## User Context

**When it happens:** Reacting to WhatsApp messages  
**Frequency:** After message send, inconsistent  
**Impact:** Can't react to messages, partial functionality

## PM Analysis (5 Whys)

**Why react fail?** → WhatsApp Web listener gone  
**Why gone?** → Session timed out or unstable  
**Why unstable?** → Baileys library connection issues  
**Why issues?** → Long-running connections not maintained  
**Root cause:** WhatsApp Web connection not properly maintained

## Classification

- **Type:** WhatsApp integration issue
- **Severity:** Medium (reactions broken, messages work)
- **Category:** Connection management

## Fix Steps

### Workaround 1: Send react first, then message
```javascript
// React before sending content
await openclaw channels whatsapp react --message-id <id> 👍
```

### Workaround 2: Re-authenticate WhatsApp
```bash
# Re-link WhatsApp
openclaw channels whatsapp link
```

### Workaround 3: Use polling mode
```json
{
  "channels": {
    "whatsapp": {
      "mode": "polling"
    }
  }
}
```

## Prevention

**For users:**
- Keep WhatsApp connection active
- Monitor connection status
- Re-authenticate periodically

**For OpenClaw team:**
- Improve Baileys connection stability
- Auto-reconnect on react failure
- Better connection state management

## User-Friendly Explanation

> "Your WhatsApp connection gets weak when you try to react to messages — it's like your phone loses signal when you try to do two things at once. React to the message quickly after receiving it, or re-link your WhatsApp to get a fresh connection."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#51682](https://github.com/openclaw/openclaw/issues/51682)