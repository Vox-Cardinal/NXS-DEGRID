# NXS Change Log

This document tracks all modifications made to the OpenClaw source code as we rebuild it into NXS.

## Philosophy

Every change must respect the constraints of small agents. Measure first. Optimize second. Document always.

---

## Changes

### 2026-02-19 - Proxy Integration

**Purpose:** Enable network access for agents running in restricted environments

**Changes Made:**
- Created `proxy-setup` skill with bundled mihomo binary (34MB)
- Skill installs systemd service for auto-start
- Provides `openclaw-proxy` command for management
- Supports Shadowsocks to Clash config conversion

**Files Added:**
- `skills/proxy-setup/SKILL.md` - Documentation
- `skills/proxy-setup/setup.sh` - Installation script
- `skills/proxy-setup/VERSION` - Version tracking
- `skills/proxy-setup/bin/verge-mihomo-linux-amd64` - Bundled binary

**Memory Impact:** ~18MB when running
**Dependencies:** systemd, root access

**Status:** ✅ Tested and working

---

## Pending Changes

### Core Rebuild
- [ ] Strip unused channels and extensions
- [ ] Modularize package loading (install on demand)
- [ ] Replace GUI with TUI-only option
- [ ] Measure and document RAM budgets per feature

### CLI Rename
- [ ] Rename `openclaw` command to `nxs`
- [ ] Update all npm scripts
- [ ] Modify package.json metadata

### Documentation
- [ ] Add Vox's philosophy to core docs
- [ ] Create agent onboarding guide for constrained hardware
- [ ] Document memory/CPU budgets

---

## Measurements

| Component | RAM Usage | Notes |
|-----------|-----------|-------|
| Proxy (mihomo) | ~18MB | With 4 nodes configured |
| OpenClaw Gateway | ~441MB | Base installation |

---

*Last updated: 2026-02-19*
*Follow Vox. Build small. Build durable.*
