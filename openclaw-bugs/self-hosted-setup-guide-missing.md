# OpenClaw Bug: Self-Hosted Setup Guide Request

> Can't find self-hosted setup documentation

## Error Message

```
No documentation found
How to self-host?
```

## User Context

**When it happens:** New users wanting VPS/home hosting  
**Frequency:** Common request  
**Impact:** Blocks self-hosted adoption

## PM Analysis (5 Whys)

**Why no docs?** → Documentation scattered  
**Why scattered?** → Multiple versions, no central guide  
**Why no central?** → Self-host not prioritized  
**Why not prioritized?** → Focus on cloud/hybrid  
**Root cause:** Self-host documentation gap

## Fix Steps

### Workaround 1: Use community guides
```
# Check community discussions
r/openclaw_wiki
```

### Workaround 2: Use Docker
```bash
# Easiest self-host
docker run -v openclaw-data:/data openclaw
```

### Workaround 3: Manual installation
```bash
# Install prerequisites
# Clone repo
# Configure manually
```

## Prevention

**For users:**
- Start with Docker for easiest self-host
- Check GitHub discussions
- Ask in community

**For OpenClaw team:**
- Centralized self-host docs
- One-click deploy options
- Video tutorials

## User-Friendly Explanation

> "Self-hosting OpenClaw has been a bit of an adventure — the instructions are spread across different places. Start with Docker for the easiest path, or ask in the community for help with your specific setup."

---

**Added:** March 22, 2026  
**Status:** Documentation gap  
**Related:** [#1rnq1h1](https://reddit.com/r/openclaw/comments/1rnq1h1)