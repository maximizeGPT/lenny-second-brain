# OpenClaw Bug: Ollama Models Hang

> Ollama LLM requests never complete

## Error Message

```
LLM request timed out
```

## User Context

**When it happens:** Using Ollama local models  
**Frequency:** Consistent on 2026.3.8+  
**Impact:** Can't use local LLMs, forced to use cloud APIs

## PM Analysis (5 Whys)

**Why hang?** → Request never returns  
**Why no return?** → Ollama API changed response format  
**Why changed?** → Ollama updated, OpenClaw didn't adapt  
**Why not adapted?** → Version mismatch in integration  
**Root cause:** API drift between Ollama and OpenClaw

## Classification

- **Type:** Integration/compatibility issue
- **Severity:** Medium (local LLM users affected)
- **Category:** LLM provider integration

## Fix Steps

### Fix 1: Downgrade Ollama
```bash
# Use Ollama version compatible with OpenClaw
ollama --version  # Check
# Downgrade to 0.1.24 or earlier
```

### Fix 2: Use OpenAI-compatible endpoint
```json
{
  "models": {
    "ollama": {
      "baseUrl": "http://localhost:11434/v1",
      "api": "openai-completions"
    }
  }
}
```

### Fix 3: Switch to cloud provider temporarily
```json
{
  "models": {
    "default": "moonshot/kimi-k2.5"
  }
}
```

## Prevention

**For users:**
- Pin Ollama version after testing
- Monitor OpenClaw release notes for Ollama updates
- Have fallback cloud provider configured

**For OpenClaw team:**
- Test against latest Ollama before releases
- Support multiple Ollama API versions
- Better error messages for version mismatch

## User-Friendly Explanation

> "Your local AI (Ollama) updated and changed how it talks to OpenClaw. Now they can't understand each other. Either downgrade Ollama to an older version, or switch to a cloud AI service until OpenClaw catches up."

---

**Added:** March 22, 2026  
**Status:** Version compatibility issue  
**Related:** [#41871](https://github.com/openclaw/openclaw/issues/41871)
