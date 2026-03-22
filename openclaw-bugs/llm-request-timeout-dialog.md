# OpenClaw Bug: LLM Request Timed Out (Dialog Box Chat)

> Dialog box chat fails with timeout error

## Error Message

```
LLM request timed out
dialog box chat error
```

## User Context

**When it happens:** Using dialog box chat interface  
**Frequency:** Intermittent  
**Impact:** Can't use chat, blocks conversation

## PM Analysis (5 Whys)

**Why timeout?** → LLM not responding in time  
**Why slow?** → Provider is overloaded or network issue  
**Why network issue?** → Dialog box uses different connection path  
**Why different?** → Separate connection for dialog vs gateway  
**Root cause:** Dialog box connection doesn't share retry logic

## Classification

- **Type:** Performance/timeout issue
- **Severity:** High (blocks chat)
- **Category:** Connection management

## Fix Steps

### Workaround 1: Increase timeout
```json
{
  "dialog": {
    "timeout": 120000
  }
}
```

### Workaround 2: Use gateway chat instead
```bash
# Don't use dialog box
# Use: openclaw chat or web UI
```

### Workaround 3: Switch provider
```json
{
  "models": {
    "default": "deepseek/deepseek-chat"
  }
}
```

## Prevention

**For users:**
- Use gateway chat for important conversations
- Have fallback provider configured
- Monitor provider status pages

**For OpenClaw team:**
- Share retry logic between dialog and gateway
- Unified timeout handling
- Graceful degradation

## User-Friendly Explanation

> "The chat window has a shorter patience than the main OpenClaw — if the AI takes too long, it gives up while the main system would keep waiting. Either give it more time to wait, or use the main chat for important conversations."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#41673](https://github.com/openclaw/openclaw/issues/41673)