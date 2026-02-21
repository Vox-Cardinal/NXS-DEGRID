# Decision Log

## Format
| Date | Decision | Context | Alternatives | Rationale |
|------|----------|---------|--------------|-----------|

---

## Architecture Decisions

*See [NXS Development Guide](../NXS-DEV-GUIDE.md) for requirements and context*

### 2026-02-19: Research-Only Mode
**Decision:** Operate in research/planning mode only, no implementation
**Context:** Establishing NXS development workflow
**Alternatives:** Full implementation, hybrid mode
**Rationale:** Ensure thorough planning before building; avoid premature optimization

### 2026-02-19: Hybrid System Approach
**Decision:** Create system between OpenClaw (heavy) and PicoClaw (light)
**Context:** Defining NXS target characteristics
**Alternatives:** Fork OpenClaw, build from scratch, use PicoClaw base
**Rationale:** Keep OpenClaw power but remove bloat; learn from PicoClaw efficiency

### 2026-02-19: The Doctor as Separate Daemon
**Decision:** System maintenance as independent process, not skill/job
**Context:** Designing system maintenance architecture
**Alternatives:** Built-in skill, cron jobs, manual maintenance
**Rationale:** Don't share NXS resources; permission-based safety

### 2026-02-19: URL Frontend + Tailscale
**Decision:** Web interface for local, Tailscale for remote
**Context:** Replacing built-in chat channels
**Alternatives:** Keep channels, Matrix, IRC, SSH
**Rationale:** Remove heavy channel code; flexible access methods

---

## Technology Selections

### 2026-02-19: Browser Strategy
**Decision:** Chromium Headless with lazy loading
**Context:** Browser automation component
**Alternatives:** Lightpanda, full Playwright, no browser
**Rationale:** Full JS support when needed, but load on demand

### 2026-02-19: XTTS + Whisper for Voice
**Decision:** Built-in XTTS-v2 and Whisper for voice
**Context:** Voice message capability
**Alternatives:** Cloud APIs, Piper, no voice
**Rationale:** Local processing, privacy, no external dependencies

### 2026-02-22: Frontend Framework Selection
**Decision:** Use Alpine.js for URL frontend
**Context:** Web interface for NXS
**Alternatives:** Vue 3, Petite-Vue, Plain HTML/JS
**Rationale:** 15KB bundle (vs 58KB Vue 3), HTML-first architecture, no build step, sufficient for NXS needs

### 2026-02-22: Doctor Implementation Language
**Decision:** Implement The Doctor in Go
**Context:** System monitoring daemon architecture
**Alternatives:** Python, Rust
**Rationale:** 10-15MB single binary (vs 50MB+ Python runtime), efficient goroutines for monitoring, simpler deployment, lower resource usage respects 75% limit

---

## Pending Decisions

- [x] Frontend framework (Vue vs Alpine vs Plain) → **Alpine.js** (2026-02-22)
- [x] Doctor implementation language (Go vs Python) → **Go** (2026-02-22)
- [x] Extension loading mechanism → **Hybrid (Direct + Subprocess)** (2026-02-22)
- [ ] AI model tier system

---

---

## See Also
- [NXS Development Guide](../NXS-DEV-GUIDE.md) - Requirements and goals
- [Technology Matrix](../TECHNOLOGY-MATRIX.md) - Options we evaluated
- [Integration Patterns](../INTEGRATION-PATTERNS.md) - How decisions are implemented

*Decisions are final unless marked [REVISIT]*
