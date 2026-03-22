# OpenClaw Bug: MiniMax Streaming Not Working

> MiniMax models don't stream responses

## Error Message

```
Response received (no streaming)
```

## User Context

**When it happens:** Using MiniMax models with streaming enabled  
**Frequency:** Consistent  
**Impact:** Slow perceived response, poor UX

## PM Analysis (5 Whys)

**Why no stream?** → MiniMax API doesn't return chunks  
**Why not chunks?** → OpenClaw not using streaming endpoint  
**Why wrong endpoint?** → MiniMax has separate streaming API  
**Why not implemented?** → Integration uses non-streaming API  
**Root cause:** MiniMax integration missing streaming support

## Classification

- **Type:** Feature gap/integration issue
- **Severity:** Low (works, just slow)
- **Category:** LLM provider integration

## Fix Steps

### Workaround 1: Use different provider
```json
{
  "models": {
    "default": "moonshot/kimi-k2.5"
  }
}
```

### Workaround 2: Disable streaming for MiniMax
```json
{
  "models": {
    "minimax": {
      "streaming": false
    }
  }
}
```

### Workaround 3: Wait for full response
```bash
# Just be patient
# Response comes, just not streamed
```

## Prevention

**For users:**
- Use streaming-capable providers for interactive use
- Disable streaming for MiniMax
- Check provider docs for streaming support

**For OpenClaw team:**
- Implement MiniMax streaming API
- Document which providers support streaming
- Graceful fallback to non-streaming

## User-Friendly Explanation

> "MiniMax sends the whole response at once instead of showing it as it types. It's like getting a letter instead of a text message — same info, just slower to arrive. Use a different AI provider if you want to see responses as they're generated."

---

**Added:** March 22, 2026  
**Status:** Feature gap, workarounds available  
**Related:** [#45882](https://github.com/openclaw/openclaw/issues/45882)
