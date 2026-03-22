# OpenClaw Bug: Node Llama CPP Build Failure

> node-llama-cpp fails to install

## Error Message

```
node-llama-cpp build failure
make: *** [ Makefile: No such file or directory ]
```

## User Context

**When it happens:** Installing OpenClaw with local model support  
**Frequency:** Platform-dependent  
**Impact:** Can't use local llama models

## PM Analysis (5 Whys)

**Why failure?** → C++ build tools missing  
**Why missing?** → Not installed on system  
**Why not installed?** → Not in default installation  
**Why not included?** → Large dependency, platform-specific  
**Root cause:** Build dependencies not auto-installed

## Classification

- **Type:** Installation/dependency issue
- **Severity:** Medium (blocks local models)
- **Category:** Setup

## Fix Steps

### Fix 1: Install build tools
```bash
# macOS
brew install cmake make gcc

# Ubuntu/Debian
sudo apt-get install build-essential cmake

# Windows
# Install Visual Studio Build Tools
```

### Fix 2: Use pre-built binaries
```bash
# Instead of building from source
# Download pre-built node-llama-cpp binaries
```

### Fix 3: Disable node-llama-cpp
```json
{
  "models": {
    "llama": {
      "enabled": false
    }
  }
}
```

## Prevention

**For users:**
- Install build tools before OpenClaw
- Check platform requirements first
- Use cloud models if local fails

**For OpenClaw team:**
- Better dependency detection
- Pre-built binaries for common platforms
- Clear error messages for missing tools

## User-Friendly Explanation

> "OpenClaw needs special programming tools to build the local AI model support, and your computer doesn't have them. Either install the build tools (like installing software before using it), or just use cloud AI models instead."

---

**Added:** March 22, 2026  
**Status:** Installation issue, workarounds available  
**Related:** [#41819](https://github.com/openclaw/openclaw/issues/41819)