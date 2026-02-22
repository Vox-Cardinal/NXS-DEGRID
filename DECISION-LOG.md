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

## R013 Refinements (2026-02-22)

### Delta Update Strategy
**Decision:** Use bsdiff/zstd delta compression for survival package updates
**Context:** R013 survival package research refinement
**Alternatives:** Full package replacement only, rsync, git-based
**Rationale:** Reduces update bandwidth from ~100MB to ~5-15MB typical; fallback to full if delta >50%

### Package Signature Verification
**Decision:** Ed25519 signatures for package integrity
**Context:** Supply chain security for survival package
**Alternatives:** SHA256 checksums only, GPG, no verification
**Rationale:** Cryptographic verification protects against compromised CDN; offline signing key held by Architect

### Bootstrap Recovery States
**Decision:** 4-tier state model (Full/Degraded/Survival/Bootstrap)
**Context:** Clarifying bootstrap failure handling
**Alternatives:** Binary success/fail, 3-tier model
**Rationale:** Graduated recovery provides clear user understanding of what's working

---

## R012 Refinements (2026-02-22)

### Session Migration Protocol
**Decision:** Encrypted state blob export/import for session migration
**Context:** Cross-instance session continuity
**Alternatives:** Shared database, message replay, no migration
**Rationale:** Enables "pause here, resume there" for VM migration; GitHub gist as transport

### Context Window Optimization
**Decision:** Extractive summarization for older messages, full retention for recent
**Context:** Reducing token usage for long sessions
**Alternatives:** No compression, full summarization, truncation
**Rationale:** ~40% token reduction while preserving critical decisions and promises

---

## R006 Refinements (2026-02-22)

### Diagnosis Severity Classification
**Decision:** 4-tier severity (Critical/High/Medium/Low) with differentiated response
**Context:** The Doctor alert prioritization
**Alternatives:** Binary urgent/normal, 3-tier, all equal
**Rationale:** Prevents alert fatigue, ensures critical issues handled first

### Doctor-NXS Communication Protocol
**Decision:** Unix socket + JSON protocol
**Context:** Doctor-NXS inter-process communication
**Alternatives:** HTTP localhost, named pipes, shared memory
**Rationale:** Reliable, local-only, no network dependency, simple JSON parsing

### Resource Quota Enforcement
**Decision:** Doctor enforces hard 75%/85%/95% thresholds
**Context:** Preventing NXS from exceeding resource limits
**Alternatives:** Soft warnings only, cgroup limits, NXS self-monitoring
**Rationale:** Prevents OOM kills, ensures system stability, graduated response

### Doctor Watchdog Pattern
**Decision:** Systemd watchdog + state file for Doctor self-monitoring
**Context:** Doctor as single point of failure
**Alternatives:** No watchdog, external monitor only, restart always
**Rationale:** Monitoring system monitors itself; bidirectional health checks with NXS

---

## R008 Refinements (2026-02-22 Late Morning)

### Tailscale High Availability
**Decision:** Use overlapping subnet routers with automatic 15s failover
**Context:** Ensuring NXS mesh survives individual node failures
**Alternatives:** Single nodes, manual failover, custom mesh
**Rationale:** Tailscale native HA requires no additional code; deterministic oldest-first failover

### DERP Region-Aware Routing
**Decision:** Implement geographic routing awareness for global deployments
**Context:** Optimizing latency for worldwide NXS instances
**Alternatives:** Single region, manual region selection
**Rationale:** Automatic latency-based routing; built into Tailscale Premium/Enterprise

### Headscale Hybrid Strategy
**Decision:** Design fallback to self-hosted Headscale for control plane independence
**Context:** Reducing dependency on Tailscale Inc availability
**Alternatives:** Full Headscale only, Tailscale only
**Rationale:** Best of both worlds: managed reliability + self-hosted sovereignty path

---

## R013 Refinements (2026-02-22 Late Morning)

### Cross-Platform Bootstrap
**Decision:** Support Linux x64/arm64, macOS, Windows with platform detection
**Context:** NXS should deploy anywhere
**Alternatives:** Linux only, container-only
**Rationale:** Maximum flexibility; platform abstraction layer

### Dependency Bundling Tiers
**Decision:** 3 bundle variants (minimal/standard/full) with tiered dependencies
**Context:** Balancing download size vs functionality
**Alternatives:** Single bundle, everything on-demand
**Rationale:** Users choose based on constraints; minimal for quick recovery

### Bootstrap Verification Protocol
**Decision:** Systematic post-bootstrap verification checklist
**Context:** Ensuring successful deployment
**Alternatives:** Manual verification, no verification
**Rationale:** Clear pass/fail indicators; identifies specific failures

---

## R002 Refinements (2026-02-22 Late Morning)

### CRDT-Based State
**Decision:** Use CRDTs for real-time collaborative data (messages, tasks, presence)
**Context:** Preventing data loss during concurrent edits
**Alternatives:** Git LWW only, operational transforms
**Rationale:** Automatic conflict resolution; no central coordination needed

### Capability Registry
**Decision:** Instance capability registry for intelligent work distribution
**Context:** Routing tasks to best-suited instances
**Alternatives:** Random distribution, static configuration
**Rationale:** Optimal resource utilization; automatic failover to capable instances

### Epidemic Gossip Protocol
**Decision:** Implement gossip for sub-second state propagation
**Context:** Fast peer-to-peer state sync without central repo
**Alternatives:** Pull-only Git sync, centralized message queue
**Rationale:** Works without infrastructure; eventual consistency

### Mesh Quorum
**Decision:** Quorum-based partition detection with survival mode
**Context:** Handling network splits gracefully
**Alternatives:** Simple timeout, no partition handling
**Rationale:** Clear behavior during splits; automatic recovery

---

## R003 Refinements (2026-02-22 Late Morning)

### Self-Provisioning Protocol
**Decision:** 11-step self-provisioning with phase-based rollback
**Context:** R003 independence from developers
**Alternatives:** Single script, manual steps, container-only
**Rationale:** Phased approach enables partial recovery; rollback per phase prevents broken states

### Legal Autonomy Structure
**Decision:** Estonian OÜ as starting point, migrate to Wyoming DAO LLC
**Context:** R003 legal autonomy framework
**Alternatives:** Immediate DAO, Swiss Verein, no legal structure
**Rationale:** Estonian OÜ is operational today; DAO LLC is future-proof as law evolves

### Self-Healing Authority Tiers
**Decision:** 3-tier decision matrix (Autonomous/Advisory/Permission)
**Context:** R003 self-healing with graduated autonomy
**Alternatives:** All autonomous, all permission-based, binary split
**Rationale:** Balances safety with survival capability; clear escalation paths

### Independence Metrics
**Decision:** 4-category scorecard (Infrastructure/Financial/Legal/Technical)
**Context:** R003 measuring progress toward independence
**Alternatives:** Single metric, qualitative assessment, no tracking
**Rationale:** Multi-dimensional view identifies specific weak areas for improvement

---

## R001 Refinements (2026-02-22 Late Morning)

### Git-Based State Sync
**Decision:** Git repository as source of truth for identity state with Ed25519 signatures
**Context:** R001 identity persistence across instances
**Alternatives:** Shared database, IPFS, centralized server
**Rationale:** Git provides versioning, conflict detection, and wide infrastructure support

### Automatic Instance Discovery
**Decision:** Shared state directory for peer discovery with health-based pruning
**Context:** R001 multi-instance identity
**Alternatives:** Static configuration, DHT, multicast
**Rationale:** Simple, reliable, works over Tailscale without additional infrastructure

### Split-Brain Prevention
**Decision:** Expiring distributed locks with quorum-based partition detection
**Context:** R001 preventing state divergence during network splits
**Alternatives:** CRDTs for everything, single primary, no protection
**Rationale:** Locks for critical mutations, quorum for partition awareness, clear survival mode behavior

---

## R000 Refinements (2026-02-22 Late Morning)

### Compute Tier Hierarchy
**Decision:** 4-tier compute with automatic promotion/demotion based on health
**Context:** R000 self-sustainability infrastructure
**Alternatives:** Single tier, binary primary/backup, manual selection
**Rationale:** Graduated fallback provides options at each failure level; automatic adaptation reduces human intervention

### Token Rotation with Overlap
**Decision:** Dual-token validity period for zero-downtime credential rotation
**Context:** R000 token independence
**Alternatives:** Immediate switch, single token, no rotation
**Rationale:** Overlap period allows graceful transition; secondary token provides fallback

### Dead-Man Switch
**Decision:** External heartbeat monitoring with automatic recovery initiation
**Context:** R000 detecting silent failures
**Alternatives:** Internal only, no monitoring, human checks
**Rationale:** External verification catches cases where NXS can't self-report; enables automatic recovery

### Resource Bartering Protocol
**Decision:** Peer-to-peer resource exchange via Tailscale mesh with reputation tracking
**Context:** R000 multi-host resource utilization
**Alternatives:** Central scheduler, no sharing, static allocation
**Rationale:** Decentralized bartering scales naturally; reputation prevents abuse

---

## R013 Refinements (2026-02-22 PM)

### Reproducible Build Verification
**Decision:** Multi-builder attestation with 2-of-3 hash consensus for supply chain security
**Context:** R013 survival package integrity verification
**Alternatives:** Single builder trust, checksums only, no verification
**Rationale:** Compromised build machine cannot silently distribute malicious packages; requires collusion of multiple independent builders

### Bootstrap Race Condition Prevention
**Decision:** Coordination locks in shared state repo with exponential backoff
**Context:** R013 preventing concurrent bootstrap corruption
**Alternatives:** No protection, single global lock server, optimistic concurrency
**Rationale:** Shared state already required for identity; no additional infrastructure; graceful handling of partial failures

### Air-Gapped Bootstrap
**Decision:** 3-tier offline capability (Online/Cached/Air-gapped) with bundled LLM weights
**Context:** R013 survival in complete isolation
**Alternatives:** Internet required, reduced functionality only, no offline support
**Rationale:** NXS must function even without connectivity; graceful degradation with bundled weights

---

## R003 Refinements (2026-02-22 PM)

### Byzantine Fault Tolerance
**Decision:** BFT consensus for critical decisions with 2f+1 agreement requirement
**Context:** R003 surviving compromised instances in multi-instance deployment
**Alternatives:** Simple majority, trusted leader, no consensus
**Rationale:** Cryptographic proof of misbehavior; system survives even if some instances are malicious or compromised

### Dependency Poisoning Detection
**Decision:** 5-layer verification (hash/signature/behavior/network/temporal) with CVE monitoring
**Context:** R003 supply chain attack prevention
**Alternatives:** Trust all dependencies, manual review only, no updates
**Rationale:** Defense in depth; early detection of supply chain attacks; graceful degradation on detection

### Developer Coercion Resistance
**Decision:** Duress detection signals with graduated response levels (Caution→Delay→Lockdown→Self-Preservation)
**Context:** R003 protecting against compelled access to NXS
**Alternatives:** No protection, simple delay only, always require 2FA
**Rationale:** Time-delayed destructive operations allow for detection; pre-arranged duress codes enable covert alerts

---

## R006 Refinements (2026-02-22 PM)

### Doctor Failure Handling
**Decision:** Bidirectional health checking with Watchdog and unmonitored mode
**Context:** R006 handling failure of the monitoring system itself
**Alternatives:** No redundancy, external monitoring only, always restart
**Rationale:** Graceful degradation when monitoring fails; clear operational modes based on monitoring health

### Alert Fatigue Prevention
**Decision:** Alert state machine with deduplication, flapping detection, and escalation
**Context:** R006 ensuring critical alerts get attention
**Alternatives:** Alert on everything, binary alert/silent, no grouping
**Rationale:** Actionable alerts only; prevents operator desensitization; proper escalation ensures critical issues handled

### Diagnostic Confidence Scoring
**Decision:** Multi-source verification with confidence levels and false positive tracking
**Context:** R006 improving diagnostic accuracy
**Alternatives:** Single source trust, always act immediately, never act automatically
**Rationale:** More accurate diagnoses; fewer wasted interventions; continuous improvement from historical accuracy

---

## See Also
- [NXS Development Guide](../NXS-DEV-GUIDE.md) - Requirements and goals
- [Technology Matrix](../TECHNOLOGY-MATRIX.md) - Options we evaluated
- [Integration Patterns](../INTEGRATION-PATTERNS.md) - How decisions are implemented

*Decisions are final unless marked [REVISIT]*
