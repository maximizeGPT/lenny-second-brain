# OpenClaw Bug: Custom Skills Not Discovered

> Skills in `~/.openclaw/workspace/skills/` not found by CLI

## Error Message

```
Skill not found
Command not recognized
```

## User Context

**When it happens:** After creating custom skills in workspace `skills/` folder  
**Frequency:** Consistent on 2026.3.13  
**Impact:** Can't use custom skills, blocks workflow customization

## PM Analysis (5 Whys)

**Why skill not found?** → CLI doesn't see the skill  
**Why not seen?** → Skills discovery ignores `extraDirs` config  
**Why ignored?** → 2026.3.13 regression in pi-coding-agent dependency  
**Why regression?** → Two separate lookup paths that don't share configuration  
**Root cause:** Skills discovery system has fragmented path resolution

## Classification

- **Type:** Configuration/expectation mismatch
- **Severity:** High (breaks custom workflows)
- **Category:** Discovery/path issue

## Fix Steps

### Workaround 1: Move to `references/`
```bash
# Instead of:
~/.openclaw/workspace/skills/my-skill/

# Use:
~/.openclaw/workspace/references/my-skill/
```

### Workaround 2: Symlink (may not work)
```bash
ln -s ~/.openclaw/workspace/skills ~/.openclaw/skills
```

### Workaround 3: Downgrade to 2026.3.12
```bash
npm install -g openclaw@2026.3.12
```

## Prevention

**For users:**
- Use `references/` folder for custom skills (safer)
- Check OpenClaw version before creating skills
- Test skill discovery after each update

**For OpenClaw team:**
- Unify skills discovery paths
- Document the `references/` vs `skills/` distinction
- Add validation: warn if skills in wrong location

## User-Friendly Explanation

> "OpenClaw looks for skills in a different place than where you're putting them. It's confusing — the docs say `skills/` folder but the CLI actually checks `references/` folder. Move your skills there and they should work."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#49873](https://github.com/openclaw/openclaw/issues/49873)
