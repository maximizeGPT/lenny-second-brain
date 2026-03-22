# OpenClaw Bug: Config File Not Found (New Users)

> Can't find openclaw.json config file

## Error Message

```
Config file not found
openclaw.json missing
```

## User Context

**When it happens:** New users setting up OpenClaw  
**Frequency:** Common for new installs  
**Impact:** Can't configure OpenClaw

## PM Analysis (5 Whys)

**Why not found?** → Config file doesn't exist by default  
**Why not created?** → No default config on fresh install  
**Why no default?** → Assumed users know to create  
**Why assumed?** → v1 had different setup  
**Root cause:** No onboarding config created automatically

## Fix Steps

### Fix 1: Generate default config
```bash
openclaw init
# Creates openclaw.json with defaults
```

### Fix 2: Copy template
```bash
cp ~/.openclaw/templates/openclaw.json ~/.openclaw/
```

### Fix 3: Use environment variables
```bash
# Instead of config file
export OPENCLAW_MODEL="moonshot/kimi-k2.5"
```

## Prevention

**For users:**
- Run `openclaw init` after install
- Check docs for config location
- Use environment variables for quick start

**For OpenClaw team:**
- Create default config on first run
- Better onboarding flow
- Clear error message with fix

## User-Friendly Explanation

> "OpenClaw doesn't create a config file automatically — it's like getting a new phone without any settings. Run 'openclaw init' to create a starter config, or copy one from the docs."

---

**Added:** March 22, 2026  
**Status:** Onboarding issue, fix available  
**Related:** [#1j8k2n2](https://reddit.com/r/openclaw/comments/1j8k2n2)