# OpenClaw Bug: Rate Limits with Free Providers

> Free LLM providers hit rate limits

## Error Message

```
Rate limit exceeded
429 Too Many Requests
```

## User Context

**When it happens:** Using free tier LLM providers  
**Frequency:** With heavy usage  
**Impact:** Can't continue until limit resets

## PM Analysis (5 Whys)

**Why rate limited?** → Too many requests in time window  
**Why too many?** → No request throttling  
**Why no throttling?** → Default config optimized for paid tiers  
**Why paid default?** → Free tiers usually for testing only  
**Root cause:** No built-in rate limit handling for free providers

## Classification

- **Type:** Rate limiting/resource issue
- **Severity:** Medium (temporary block)
- **Category:** API throttling

## Fix Steps

### Fix 1: Add rate limiting config
```json
{
  "models": {
    "default": {
      "rateLimit": {
        "requestsPerMinute": 10,
        "retryAfter": 60000
      }
    }
  }
}
```

### Fix 2: Use multiple free providers
```json
{
  "models": {
    "fallback": ["moonshot", "deepseek", "qwen"]
  }
}
```

### Fix 3: Upgrade to paid tier
```bash
# Get API key from provider
# Configure with paid key
```

## Prevention

- Configure rate limits explicitly
- Use fallback providers
- Monitor usage with provider dashboards

## User-Friendly Explanation

> "Your free AI provider is like a free sample — after a certain amount, they cut you off. Either slow down your requests, rotate between different free providers, or upgrade to a paid plan if you need more."

---

**Added:** March 22, 2026  
**Status:** Usage limit, workarounds available  
**Related:** [#1rmc6kl](https://reddit.com/r/openclaw/comments/1rmc6kl)