# NXS TODO List

**Rule:** Items on this list are for research and planning ONLY. Do not execute without explicit permission.

---

## Network & Connectivity

### [ ] Tailscale Setup
**Status:** Research/Plan only
**Why:** Secure mesh network for remote access
**Questions to answer:**
- Is Tailscale allowed on this VM?
- What are the security implications?
- How does it interact with existing proxy?
- Resource usage (RAM/CPU)?

**Notes:**
- Would provide easy access from user's devices
- Alternative to Matrix for communication
- Requires authentication with Tailscale account

---

## Completed

### [x] Proxy Setup
**Status:** ✅ DONE
**Result:** mihomo running on 127.0.0.1:7890

### [x] Matrix Plugin Test
**Status:** ✅ CLEANED UP
**Result:** Requires interactive setup, not suitable

### [x] OpenClaw Rebrand to NXS
**Status:** ✅ IN PROGRESS
**Location:** /opt/development/NXS-Dev/

---

*Last updated: 2026-02-19*
*Follow Vox. Plan carefully. Execute only when permitted.*

---

## Optimization

### [ ] Reduce Hardware Usage
**Status:** Research/Plan only
**Goal:** Lower RAM, CPU, disk footprint
**Areas to investigate:**
- Disable unused OpenClaw channels/extensions
- Trim NXS rebrand (remove unnecessary files)
- Optimize proxy configuration
- Reduce log verbosity
- Clean up old sessions/transcripts

**Current baseline:**
- RAM: ~943MB used / 3.8GB total
- Top consumer: openclaw-gateway (~388MB)

### [ ] Reduce Token Usage
**Status:** Research/Plan only
**Goal:** Minimize API calls and context window usage
**Areas to investigate:**
- Shorter, more focused prompts
- Better context management (memory files)
- Batch related tasks
- Use cheaper models for simple tasks
- Reduce heartbeat frequency
- Compress old conversation history

**Current model:** kimi-coding/k2p5
