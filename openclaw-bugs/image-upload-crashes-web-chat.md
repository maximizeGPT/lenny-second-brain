# OpenClaw Bug: Image Upload Crashes Web Chat

> Web chat crashes after sending image

## Error Message

```
Web chat crashed
Session file corrupted
```

## User Context

**When it happens:** Sending images via web chat interface  
**Frequency:** Consistent with certain image types  
**Impact:** Data loss, session corruption, can't use images

## PM Analysis (5 Whys)

**Why crash?** → Image processing fails  
**Why fail?** → File size or format not validated  
**Why not validated?** → Missing input sanitization  
**Why missing?** → Assumed client-side validation  
**Root cause:** No server-side image validation

## Classification

- **Type:** Data integrity/crash issue
- **Severity:** High (data corruption)
- **Category:** Input validation

## Fix Steps

### Workaround 1: Resize images first
```bash
# Resize to under 1MB
convert image.png -resize 50% small-image.png
```

### Workaround 2: Use URL instead
```
# Upload to imgur/S3 first
# Send URL instead of file
```

### Workaround 3: Use CLI upload
```bash
# Bypass web UI
openclaw channels upload --file image.png
```

## Prevention

**For users:**
- Resize images before uploading
- Use external image hosting
- Backup sessions before image uploads

**For OpenClaw team:**
- Validate image size/format server-side
- Graceful error handling for bad images
- Automatic image resizing

## User-Friendly Explanation

> "Your image was too big or in a weird format and it crashed the chat. It's like trying to mail a package that's too heavy — the system breaks. Resize your images first or upload them somewhere else and just send the link."

---

**Added:** March 22, 2026  
**Status:** Active issue, workarounds available  
**Related:** [#51669](https://github.com/openclaw/openclaw/issues/51669)
