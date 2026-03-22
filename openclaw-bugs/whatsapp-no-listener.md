# OpenClaw Bug: WhatsApp "No Active Listener"

> WhatsApp Web connection drops after QR code scan

## Error Message

```
No active WhatsApp Web listener
401 Unauthorized
```

## User Context

**When it happens:** After scanning QR code, connection drops within seconds  
**Frequency:** Consistent on 2026.3.13, affects Windows/WSL2/Linux  
**Impact:** Can't use WhatsApp integration at all

## PM Analysis (5 Whys)

**Why no listener?** → WhatsApp Web connection closed  
**Why did it close?** → Baileys library session invalid  
**Why invalid?** → 2026.3.13 changed session handling  
**Why didn't old sessions work?** → Breaking change in auth flow  
**Root cause:** Version regression in WhatsApp/Baileys integration

## Classification

- **Type:** Integration failure
- **Severity:** High (breaks WhatsApp channel)
- **Category:** Third-party API change

## Fix Steps

### Workaround 1: Wait & Retry
```bash
# After scanning QR, wait 30 seconds before sending
openclaw channels status --probe
# Look for "connected" not just "linked"
```

### Workaround 2: Clean Slate
```bash
# Unlink and re-link completely
openclaw channels disconnect whatsapp
# Delete WhatsApp session data
rm -rf ~/.openclaw/whatsapp-session
# Re-link via QR code
openclaw channels connect whatsapp
```

### Workaround 3: Downgrade (if critical)
```bash
# Use 2026.3.12 where WhatsApp is stable
npm install -g openclaw@2026.3.12
```

## Prevention

**For users:**
- Pin working OpenClaw version for production
- Test WhatsApp integration after every update
- Monitor OpenClaw GitHub for WhatsApp-related issues

**For OpenClaw team:**
- Add WhatsApp to integration test suite
- Document breaking changes in release notes
- Provide migration guide for auth changes

## User-Friendly Explanation

> "WhatsApp changed how they handle connections, and OpenClaw 2026.3.13 hasn't caught up yet. Your phone shows 'linked' but the connection drops immediately. Try waiting 30 seconds after scanning, or use version 2026.3.12 if you need WhatsApp working right now."

---

**Added:** March 22, 2026  
**Status:** Known issue, workarounds available  
**Related:** [#51012](https://github.com/openclaw/openclaw/issues/51012), [#51111](https://github.com/openclaw/openclaw/issues/51111)
