# OpenClaw Bug: Tool Errors Leaking to Chat

> Internal errors visible to end users

## Error Message

```
Error: tool execution failed
Stack trace: ...
```

Visible in chat/channel instead of being handled gracefully.

## User Context

**When it happens:** Tool fails during conversation  
**Frequency:** When tools error  
**Impact:** Poor UX, exposes internals, looks unprofessional

## PM Analysis (5 Whys)

**Why visible?** → Error not caught and formatted  
**Why not caught?** → Missing error boundary  
**Why missing?** → Tool execution doesn't wrap errors  
**Why not wrap?** → Assumed tools would always succeed  
**Root cause:** No error handling layer between tools and chat

## Classification

- **Type:** UX/regression issue
- **Severity:** Medium (embarrassing, not blocking)
- **Category:** Error handling

## Fix Steps

### Workaround 1: Wrap tool calls
```javascript
// In your agent code
try {
  result = await tool.execute();
} catch (e) {
  return "I had trouble with that. Let me try a different approach.";
}
```

### Workaround 2: Use tool validation
```bash
# Validate before executing
openclaw tools validate <tool-name>
```

### Workaround 3: Disable problematic tools
```json
{
  "tools": {
    "enabled": ["safe-tool-1", "safe-tool-2"]
  }
}
```

## Prevention

**For users:**
- Test tools thoroughly before enabling
- Wrap tool calls in error handling
- Monitor logs for tool failures

**For OpenClaw team:**
- Add error boundaries around tool execution
- Friendly error messages for users
- Log full errors for debugging only

## User-Friendly Explanation

> "When a tool breaks, OpenClaw shows the ugly technical error message to everyone. It's like your waiter telling you the kitchen is on fire instead of just saying 'there's a delay.' You need to catch these errors and show friendly messages instead."

---

**Added:** March 22, 2026  
**Status:** UX issue, workaround available  
**Related:** [#49882](https://github.com/openclaw/openclaw/issues/49882)
