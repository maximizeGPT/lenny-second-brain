# OpenClaw Bug: OpenRouter 401 Missing Authentication

> OpenRouter API calls fail with auth error

## Error Message

```
OpenRouter 401 Missing Authentication header
```

## User Context

**When it happens:** Using OpenRouter as LLM provider  
**Frequency:** After token refresh or new setup  
**Impact:** Can't use OpenRouter models, completely blocked

## PM Analysis (5 Whys)

**Why 401?** → Authentication header missing or invalid  
**Why missing?** → Token not being sent in API requests  
**Why not sent?** → Config has `apiKey` but code expects `auth_token`  
**Why mismatch?** → Documentation vs implementation drift  
**Root cause:** Config key name inconsistency across providers

## Classification

- **Type:** Configuration/auth issue
- **Severity:** High (blocks provider usage)
- **Category:** Documentation/implementation mismatch

## Fix Steps

### Fix 1: Correct config key
```json
// Wrong:
{
  "auth": {
    "openrouter": {
      "apiKey": "sk-or-xxx"
    }
  }
}

// Right:
{
  "auth": {
    "openrouter": {
      "apiKey": "sk-or-xxx",
      "headerName": "Authorization"
    }
  }
}
```

### Fix 2: Use direct OpenAI format
```json
{
  "models": {
    "openrouter": {
      "baseUrl": "https://openrouter.ai/api/v1",
      "api": "openai-completions"
    }
  }
}
```

### Fix 3: Environment variable
```bash
export OPENROUTER_API_KEY="sk-or-xxx"
```

## Prevention

**For users:**
- Always check provider-specific docs
- Test with `openclaw models test openrouter` first
- Use environment variables for API keys (safer)

**For OpenClaw team:**
- Standardize auth config across providers
- Better error messages: "Expected 'apiKey', found 'token'"
- Auto-detect common config mistakes

## User-Friendly Explanation

> "OpenClaw and OpenRouter aren't speaking the same language about your API key. OpenClaw calls it one thing, OpenRouter expects another. You need to tell OpenClaw exactly how to send the key to OpenRouter — it's like giving someone directions in their language, not yours."

---

**Added:** March 22, 2026  
**Status:** Config issue, workaround documented  
**Related:** [#51056](https://github.com/openclaw/openclaw/issues/51056)
