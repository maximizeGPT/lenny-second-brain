# OpenClaw Bug: Model Config Not Loading on Restart

> Gateway ignores new model settings

## Error Message

```
Model not found
Using default model
```

## User Context

**When it happens:** After changing model config and restarting gateway  
**Frequency:** Every restart  
**Impact:** Can't switch models, stuck with old config

## PM Analysis (5 Whys)

**Why old model?** → New config not loaded  
**Why not loaded?** → Config cached in sessions.json  
**Why cached?** → Session restores previous state  
**Why restore?** → Designed for continuity  
**Root cause:** Session state overrides config changes

## Classification

- **Type:** Configuration/state issue
- **Severity:** Medium (annoying workaround needed)
- **Category:** Config management

## Fix Steps

### Fix 1: Clear sessions after config change
```bash
openclaw gateway stop
openclaw sessions purge
openclaw gateway start
```

### Fix 2: Use --force flag
```bash
openclaw gateway restart --force-config
```

### Fix 3: Edit sessions.json manually
```bash
# Stop gateway
# Edit ~/.openclaw/sessions.json
# Remove model references
# Restart
```

## Prevention

**For users:**
- Always purge sessions after model changes
- Document your model config in README
- Use environment variables for model selection

**For OpenClaw team:**
- Config changes should invalidate session cache
- Add `openclaw config apply --restart` command
- Warn when config differs from session state

## User-Friendly Explanation

> "You changed your AI model settings but OpenClaw is remembering the old ones from your previous session. It's like changing your address but the mail still goes to your old house. Clear your sessions after changing settings so it starts fresh."

---

**Added:** March 22, 2026  
**Status:** Design issue, workaround available  
**Related:** [#44611](https://github.com/openclaw/openclaw/issues/44611)
