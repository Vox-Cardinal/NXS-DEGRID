# Technology Comparison Matrix

## Voice Synthesis (TTS)

| Technology | Size | Speed | Quality | License | Notes |
|------------|------|-------|---------|---------|-------|
| **XTTS-v2** | ~1GB | Medium | Excellent | Coqui MPL | Voice cloning, discontinued but open |
| **Piper** | ~100MB | Fast | Good | MIT | Lightweight, fast |
| **Coqui TTS** | ~500MB | Medium | Good | MPL | General TTS, many models |
| **espeak-ng** | ~10MB | Very Fast | Robotic | GPL | Lightweight, low quality |

**Recommendation:** XTTS-v2 for quality + voice cloning, Piper as lightweight fallback

---

## Speech Recognition (STT)

| Technology | Model Size | Speed | Accuracy | Notes |
|------------|------------|-------|----------|-------|
| **Whisper tiny** | 39MB | Fast | Basic | Good for commands |
| **Whisper base** | 74MB | Medium | Good | Recommended for NXS |
| **Whisper small** | 244MB | Slower | Better | If resources allow |
| **faster-whisper** | Same | Faster | Same | Optimized implementation |
| **whisper.cpp** | Same | Very Fast | Same | C++ implementation |

**Recommendation:** Whisper base with faster-whisper optimization

---

## Frontend Framework

| Framework | Size | Complexity | Performance | Notes |
|-----------|------|------------|-------------|-------|
| **Plain HTML/JS** | 0KB | Low | Fastest | No build step |
| **Vue 3** | ~30KB | Medium | Fast | Good balance |
| **React** | ~40KB | High | Fast | Overkill for this |
| **Svelte** | ~5KB | Medium | Fastest | Compile-time optimized |
| **Alpine.js** | ~15KB | Low | Fast | HTML-centric |

**Recommendation:** Vue 3 or Alpine.js for simplicity

---

## System Monitor (The Doctor)

| Language | Binary Size | Memory | Performance | Notes |
|----------|-------------|--------|-------------|-------|
| **Go** | ~10MB | Low | Fast | Like PicoClaw |
| **Python** | ~50MB | Medium | Medium | Easier to write |
| **Rust** | ~5MB | Low | Fastest | Complex, safe |
| **Bash** | 0MB | Minimal | Slow | Simple scripts only |

**Recommendation:** Go for efficiency, Python for speed of development

---

## Remote Access

| Solution | Setup | Security | Performance | Notes |
|----------|-------|----------|-------------|-------|
| **Tailscale** | Easy | WireGuard | Fast | Zero config, best UX |
| **WireGuard** | Medium | Excellent | Fast | Manual config |
| **ngrok** | Easy | Good | Medium | Public URLs, rate limits |
| **frp** | Medium | Good | Fast | Self-hosted server |

**Recommendation:** Tailscale for ease of use

---

## Browser Engine

| Engine | Size | JS Support | Speed | Notes |
|--------|------|------------|-------|-------|
| **Chromium Headless** | ~200MB | Full | Medium | Full compatibility |
| **Lightpanda** | ~50MB | Partial | Fast | Lightweight, limited |
| **Playwright** | ~200MB | Full | Medium | Automation focused |
| **Puppeteer** | ~200MB | Full | Medium | Similar to Playwright |

**Recommendation:** Chromium Headless with lazy loading

---

*Last updated: 2026-02-19*

## See Also
- [Decision Log](./DECISION-LOG.md) - Why we chose these technologies
- [NXS Development Guide](./NXS-DEV-GUIDE.md) - What we need to achieve
- [Integration Patterns](./INTEGRATION-PATTERNS.md) - How they fit together
