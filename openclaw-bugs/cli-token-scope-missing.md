# OpenClaw Bug: CLI Token Auth Scope Missing

> openclaw-cli reports missing scope despite token having it

## Error Message

```
Error: Missing scope operator.read
Token does not have required permissions
```

## User Context

**When it happens:** Using openclaw-cli after auth update  
**Frequency:** After 2026.3.13  
**Impact:** CLI commands fail, can't manage gateway

## PM Analysis (5 Whys)

**Why missing?** → Token scope not including required permission  
**Why not included?** → Token generated before scope update  
**Why not updated?** → Token refresh didn't include new scopes  
**Why new scopes?** → 2026.3.13 added operator.read requirement  
**Root cause:** Token migration not handling new scopes

## Classification

- **Type:** Auth/scope issue
- **Severity:** High (CLI blocked)
- **Category:** Token management

## Fix Steps

### Fix 1: Re-authenticate
```bash
openclaw auth logout
openclaw auth login
```

### Fix 2: Manually add scope to token
```json
{
  "token": {
    "scopes": ["operator.read", "operator.write", "gateway.manage"]
  }
}
```

### Fix 3: Patch distribution files
```bash
# For advanced users
# Edit gateway distribution to include scopes
# See: github.com/openclaw/openclaw/discussions/50474
```

## Prevention

**For users:**
- Re-authenticate after OpenClaw updates
- Check token permissions with: openclaw auth info

**For OpenClaw team:**
- Auto-migrate tokens on update
- Include all required scopes in token refresh
- Better error messages for missing scopes

## User-Friendly Explanation

> "Your login token is missing a permission that OpenClaw now requires. It's like your ID card — expired or missing a badge. Log out and log back in to get a fresh token with all the new permissions."

---

**Added:** March 22, 2026  
**Status:** 2026.3.13 regression, workarounds available  
**Related:** [#50474](https://github.com/openclaw/openclaw/issues/50474)