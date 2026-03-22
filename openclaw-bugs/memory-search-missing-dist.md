# OpenClaw Bug: Memory Search Tool Missing dist File

> memory_search fails to load CLI

## Error Message

```
memory_search tool fails to load
missing dist/memory-cli-*.js
```

## User Context

**When it happens:** Using memory search tool on 2026.3.13  
**Frequency:** Consistent  
**Impact:** Can't search MEMORY.md or memory files

## PM Analysis (5 Why)

**Why missing?** → CLI not bundled  
**Why not bundled?** → Build process missing dist step  
**Why missing?** → Package not properly built for release  
**Why not built?** → CI/CD missing dist artifact step  
**Root cause:** Incomplete build pipeline for memory tool

## Classification

- **Type:** Build/bundle issue
- **Severity:** High (tool broken)
- **Category:** Release process

## Fix Steps

### Workaround 1: Run memory_search from CLI directly
```bash
# Manual memory search
grep -r "query" memory/
```

### Workaround 2: Build manually
```bash
cd ~/.openclaw/skills/memory
npm run build
```

### Workaround 3: Use alternative search
```bash
# Use find + grep instead
find ~/.openclaw/workspace -name "*.md" -exec grep -l "query" {} \;
```

## Prevention

**For users:**
- Verify tools work after update
- Keep backup of memory files locally
- Use CLI for basic operations

**For OpenClaw team:**
- Add dist build step to CI/CD
- Verify all tools bundled before release
- Test tool loading on fresh install

## User-Friendly Explanation

> "The memory search tool didn't get fully installed. It's like buying a car without the engine — looks complete but doesn't work. Either rebuild it yourself or search your files the old way with grep."

---

**Added:** March 22, 2026  
**Status:** 2026.3.13 regression, workarounds available  
**Related:** [#51676](https://github.com/openclaw/openclaw/issues/51676)