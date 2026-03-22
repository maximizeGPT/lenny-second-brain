# OpenClaw Bug: Cron Sessions Hallucinating

> Cron jobs produce fake data instead of failing

## Error Message

```
Session completed successfully
```

But output contains fabricated information.

## User Context

**When it happens:** Scheduled cron jobs run  
**Frequency:** Intermittent, hard to detect  
**Impact:** Silent data corruption, false confidence in automation

## PM Analysis (5 Whys)

**Why fake data?** → Session didn't fail, produced output anyway  
**Why no failure?** → Error handling catches exceptions but continues  
**Why continue?** → Cron runner designed to be "resilient"  
**Why resilience over accuracy?** → Design choice prioritizes uptime over correctness  
**Root cause:** Cron error handling masks failures instead of surfacing them

## Classification

- **Type:** Reliability/data integrity issue
- **Severity:** Critical (silent failures are worst)
- **Category:** Error handling design

## Fix Steps

### Detection
```bash
# Add validation to cron outputs
openclaw cron run --validate-output

# Check logs for hallucination patterns
grep -i "i'm not sure\|i don't know\|fabricated" ~/.openclaw/logs/cron.log
```

### Prevention
```bash
# Add explicit failure conditions in your cron script
if [ -z "$REAL_DATA" ]; then
  echo "ERROR: No data received"
  exit 1
fi
```

## Prevention

**For users:**
- Always validate cron outputs against known good data
- Add health checks: "Did this actually happen?"
- Monitor for impossible/contradictory results

**For OpenClaw team:**
- Cron should fail loudly, not hallucinate quietly
- Add `--strict` mode that exits on any uncertainty
- Log confidence scores for cron outputs

## User-Friendly Explanation

> "Your cron job is 'succeeding' but making up answers when it doesn't know. It's like an employee who never admits they don't know something — dangerous. You need to add checks that verify the output is real, or switch to strict mode that fails instead of guessing."

---

**Added:** March 22, 2026  
**Status:** Design issue, detection workaround available  
**Related:** [#49876](https://github.com/openclaw/openclaw/issues/49876)
