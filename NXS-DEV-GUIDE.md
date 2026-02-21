# NXS Development Guide

## NXS Development Rules

### Allowed Activities
I am only permitted to:

1. **Research** - Investigate technologies, solutions, and approaches
2. **Plan** - Create and revise development plans
3. **Formulate Theories** - Develop architectural hypotheses and strategies
4. **Clean Up Work Process** - Organize, optimize, and streamline workflows
5. **Document Findings** - Record discoveries, conclusions, and decisions

### Prohibited Activities
I am NOT permitted to:

1. **Execute implementation** - No actual coding or building
2. **Install software** - No package installations or system modifications
3. **Modify running systems** - No changes to active NXS/OpenClaw instances
4. **Automated deployment** - No scripts that deploy or activate components
5. **Commit changes** - No git commits or pushes
6. **Implement** - No execution of planned features

### Operational Constraints

| Constraint | Limit | Purpose |
|------------|-------|---------|
| **Hardware Usage** | Max 75% | Preserve system resources |
| **Sub-agents** | Max 2 | Limit parallel complexity |
| **Implementation** | Zero | Research/planning only |
| **Commits** | Zero | No repository changes |

### Resource Monitoring
I must continuously monitor:
- RAM usage (keep under 75% of 3.8GB = 2.85GB)
- Disk usage (keep under 75% of 40GB = 30GB)
- CPU load (avoid sustained high usage)
- Active sub-agent count

### Documentation Standards
All work must be documented neatly:
- Clear structure and sections
- Tables for comparisons
- Checklists for tracking
- Timestamps for decisions
- Source citations where applicable

### Research Backup Strategy
**Rule:** Always back up research results accordingly.

**What to Back Up:**
- All markdown files (*.md)
- Research logs in `memory/`
- Decision logs, technology matrices, integration patterns
- Task indexes and planning documents

**What NOT to Back Up:**
| Folder | Reason |
|--------|--------|
| `NXS-2.13-REBRAND-DO-NOT-CLEAN/` | Active development codebase |
| `OpenClaw-2.13-REFERENCE-DO-NOT-CLEAN/` | Reference codebase |
| `PicoClaw-REFERENCE-DO-NOT-CLEAN/` | Reference codebase |

**Backup Method:**
- Target: Separate research folder on main branch
- Authentication: URL + token via API
- API: GitHub REST API for file operations
- Token: Stored in `~/.openclaw/credentials/github/token.json`
- Strategy: **Overwrite** — update existing files, don't version

**Exception:** Research backup pushes are permitted (not counted as implementation commits)

### Decision Process
1. Research and gather information
2. Formulate plan/theory
3. Present findings to you
4. Await your approval/permission
5. Document the decision
6. Move to next research item

---

## Research Process

### Session Workflow
Each research session follows this process:

1. **Session Start**
   - Load context files (SOUL.md, USER.md, memory files)
   - **Wait for user instruction or cron trigger** (research runs via scheduled jobs)

2. **Check Requirements**
   - Review NXS requirements from this guide
   - Identify gaps or areas needing more research

3. **Check Research Log**
   - Review `/opt/development/memory/` files for previous research
   - Check what has already been investigated
   - Identify what needs improvement or expansion

4. **Formulate Research Task**
   - Choose to either:
     - **Improve** an existing idea from previous research
     - **Find new ideas** to fulfill unmet requirements
   - Define clear research objective

5. **Execute Research**
   - Investigate within operational constraints
   - Document findings in real-time
   - Stay within resource limits

6. **Analyze & Improve**
   - Compare findings to existing plan
   - Logically refine based on what was found

7. **Take Notes & Document**
   - Record all findings in `/opt/development/memory/YYYY-MM-DD.md`
   - Update this guide with conclusions
   - Cite sources and references

8. **Schedule Next Run**
   - Research continues via cron job scheduling
   - Next iteration runs automatically

### Stopping Research
Research runs on cron schedule. User can pause by sending any message during an active research session. Research resumes on next scheduled run unless user explicitly disables the cron job.

### Research Cron Job
**Schedule:** Every 30 minutes  
**Target:** Isolated session  
**Task:** Auto-select research task (R000-R009), execute, document, backup  
**Delivery:** Announce results to user

**Continuous Improvement:**
Research never truly "finishes." Even when R000-R009 are complete, the cron job cycles back to improve existing findings, look for new optimizations, and refine previous conclusions. This is the default mode when no unplanned tasks remain.

---

## Reference Sources

This document references the following source code bases for NXS development:

### 1. OpenClaw 2.13 Reference
**Location:** `/opt/development/OpenClaw-2.13-REFERENCE-DO-NOT-CLEAN/`

The original OpenClaw 2.13 source code. This is the base that NXS was rebranded from.
- Original OpenClaw codebase
- Version 2.13 (matching running version)
- Clean, unmodified reference

### 2. PicoClaw Reference
**Location:** `/opt/development/PicoClaw-REFERENCE-DO-NOT-CLEAN/`

The PicoClaw project by Sipeed. Additional reference for lightweight implementations.
- Go-based implementation
- 34MB codebase
- Docker support
- Multi-language documentation

### 3. NXS Active Development
**Location:** `/opt/development/NXS-2.13-REBRAND-DO-NOT-CLEAN/`

The active NXS rebranded codebase.
- Rebranded from OpenClaw 2.13
- Contains browser-automation extension
- Active development workspace

## Goal: NXS Hybrid System

Create a new hybrid system that combines the best of both references:

### Target Characteristics

| Aspect | OpenClaw | PicoClaw | **NXS Target** |
|--------|----------|----------|----------------|
| **Size** | Heavy (~500MB+) | Lightweight (~34MB) | **Medium (~100-150MB)** |
| **Features** | Full-featured | Minimal | **Core + Essential** |
| **Extensions** | Many, heavy | Few, light | **Curated, modular** |
| **Browser** | Playwright (heavy) | None/light | **Lightweight + optional full** |
| **Memory** | High usage | Low usage | **Moderate, configurable** |
| **Proxy** | External | Built-in | **Built-in, optimized** |
| **AI Models** | Multiple options | Single/light | **Configurable tiers** |

### Philosophy

- **Not as massive as OpenClaw**: Remove bloat, keep only essential features
- **Not as limited as PicoClaw**: Keep power, make it modular and optional
- **Just right**: Core functionality always available, extras load on demand

### Key Principles

1. **Modularity**: Everything is optional except core
2. **Lazy loading**: Features load only when needed
3. **Resource awareness**: Respect RAM/disk constraints
4. **Graceful degradation**: Works with minimal resources
5. **User choice**: User decides what to enable

## Implementation Roadmap

### Phase 1: Core Foundation (Priority: Critical)
- [ ] Strip OpenClaw of unnecessary components
- [ ] Implement URL frontend (replace chat channels)
- [ ] Create "The Doctor" monitoring daemon
- [ ] Optimize browser automation (lazy loading)

### Phase 2: Voice Integration (Priority: High)
- [ ] Integrate XTTS-v2 API server
- [ ] Integrate Whisper for STT
- [ ] Test voice message flow
- [ ] Add voice to frontend

### Phase 3: Connectivity (Priority: Medium)
- [ ] Set up Tailscale for remote access
- [ ] Create API integration layer
- [ ] Test ComfyUI/external API connections
- [ ] Document API usage

### Phase 4: Polish & Optimization (Priority: Low)
- [ ] Review and remove unused extensions
- [ ] Optimize disk/RAM usage
- [ ] Create user documentation
- [ ] Final testing

## Current Status
- [x] Initial planning
- [x] Architecture decisions
- [x] System analysis
- [ ] Phase 1 implementation
- [ ] Phase 2 implementation
- [ ] Phase 3 implementation
- [ ] Phase 4 implementation

### What's Currently Running

| Component | Status | Size | Keep/Remove | Notes |
|-----------|--------|------|-------------|-------|
| **OpenClaw Gateway** | Running | ~484MB RAM | **Keep (Core)** | Main system, essential |
| **Mihomo Proxy** | Running | ~40MB RAM | **Keep** | Required for internet |
| **Browser Automation** | Installed | ~206MB disk | **Available** | Research tool — use anytime for docs, comparisons, screenshots |
| **Playwright** | Installed | ~206MB disk | **Optional** | Heavy, lazy load |
| **Lightpanda** | Removed | - | **Removed** | Replaced by Playwright |
| **Cron Jobs** | Active | Minimal | **Keep** | Health checks |
| **Feishu Extension** | Installed | Unknown | **Review** | Evaluate need |
| **Kimi Extensions** | Installed | Unknown | **Review** | Evaluate need |

### What We Want to Integrate

| Component | Purpose | Integration Approach |
|-----------|---------|---------------------|
| **XTTS-v2 API Server** | Text-to-speech | Built-in, core feature |
| **Whisper** | Speech-to-text | Built-in, core feature |
| **The Doctor** | System maintenance | Separate daemon, permission-based |
| **URL Frontend** | Web interface | Replace built-in channels |
| **Tailscale** | Remote access | Network layer, optional |
| **ComfyUI API** | Image workflows | External API integration |
| **PicoClaw Ideas** | Lightweight patterns | Adapt architecture |

### What to Remove (Safe)

| Component | Reason | Replacement |
|-----------|--------|-------------|
| **Built-in chat channels** | Too heavy, not needed | URL frontend |
| **Heavy browser (full Playwright)** | 622MB → 206MB optimized | Keep optimized only |
| **Unused extensions** | Bloat | Load on demand |
| **Matrix integration** | Failed, complex | Remove for now |
| **Test/coverage files** | Development only | Strip from production |

### What to Keep (Essential)

| Component | Reason |
|-----------|--------|
| **Core gateway** | NXS cannot function without |
| **Proxy (mihomo)** | Internet access required |
| **Health monitoring** | System stability |
| **Skill system** | Extension architecture |
| **Browser automation** | Web interaction (lazy load) |
| **Voice system** | Core feature (XTTS + Whisper) |

### Interface
- **No built-in chat channels** (remove from OpenClaw)
- **Local URL-based frontend** for local interfacing
- **Tailscale** for remote interfacing (secure, lightweight)
- **API Integration Layer** for external service connectivity

### API Integration
The frontend should support API integrations to expand capabilities:

**External API Examples:**
- **ComfyUI API** - Workflow automation from another local machine
- **Stable Diffusion API** - Image generation
- **Ollama API** - Local LLM integration
- **Custom services** - User-defined API endpoints

**Built-in Voice System (NXS Core)**
Voice capabilities integrated directly into NXS:
- **XTTS-v2 API Server** - Text-to-speech generation
- **Whisper** - Speech-to-text transcription
- **Purpose**: Frontend can receive voice messages (Whisper) and send voice responses (XTTS)
- **Benefit**: Natural voice interaction without external dependencies

### System Maintenance: The Doctor
A separate background process that maintains NXS efficiency and integrity.

**Characteristics:**
- **Not a skill** - runs outside NXS resource pool
- **Not a job** - persistent background daemon
- **Independent** - monitors system separately
- **Permission-based** - NXS agents review diagnosis and grant permission
- **Continuous monitoring** - always watching, acts only when permitted

**Workflow:**
1. Doctor monitors system continuously
2. Detects issue → Creates diagnosis
3. NXS agent reviews diagnosis
4. Agent grants permission → Doctor fixes issue
5. Doctor resumes monitoring

**Doctor Responsibilities:**
- RAM/CPU monitoring
- Disk cleanup
- Process zombie cleanup
- Extension health checks
- Log rotation
- Browser binary optimization
- Proxy health monitoring

---

## See Also
- [Research Task Index](./RESEARCH-TASK-INDEX.md) - Current research tasks
- [Technology Matrix](./TECHNOLOGY-MATRIX.md) - Technology comparisons
- [Decision Log](./DECISION-LOG.md) - Architecture decisions
- [Integration Patterns](./INTEGRATION-PATTERNS.md) - Architectural patterns

*Last updated: 2026-02-19*