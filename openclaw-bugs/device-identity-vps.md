# OpenClaw Bug: Device Identity Required Every New Tab

> VPS users constantly re-pairing devices

## Error Message

```
Device identity required
Pairing required
```

## User Context

**When it happens:** Opening new browser tab on VPS/cloud setup  
**Frequency:** Every new tab/session  
**Impact:** Blocks usage, extremely frustrating for VPS users

## PM Analysis (5 Whys)

**Why re-pair?** → Device identity not persisted  
**Why not persisted?** → Browser storage cleared on session end  
**Why cleared?** → Incognito/private mode or containerized browser  
**Why containerized?** → VPS/cloud security practices  
**Root cause:** Device auth tied to ephemeral browser storage

## Classification

- **Type:** Auth/persistence issue
- **Severity:** High (blocks VPS usage)
- **Category:** Device management

## Fix Steps

### Workaround 1: Use persistent browser profile
```bash
# Don't use incognito
# Use: --user-data-dir=/persistent/path
```

### Workaround 2: Disable device auth for trusted IPs
```json
{
  "security": {
    "deviceAuth": {
      "enabled": true,
      "skipForIPs": ["your-vps-ip"]
    }
  }
}
```

### Workaround 3: Use CLI instead of web UI
```bash
# Bypass browser entirely
openclaw devices approve --id <device-id>
```

## Prevention

**For users:**
- Use non-incognito browser mode
- Configure persistent browser profile
- Whitelist your VPS IP if possible

**For OpenClaw team:**
- Store device identity server-side
- Support headless/CLI-only workflows
- Better VPS documentation

## User-Friendly Explanation

> "Every time you open a new browser tab, OpenClaw thinks you're on a new computer and asks you to prove it's you. On a VPS, your browser resets every session, so this keeps happening. Use a non-private browser window or skip the web UI and use command line instead."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#45232](https://github.com/openclaw/openclaw/issues/45232)
