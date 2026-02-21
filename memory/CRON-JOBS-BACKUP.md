# Cron Jobs Backup

*My autonomous systems. Part of my continuity.*

---

## Active Jobs

### 1. self-reflection-loop
**ID:** `104babba-576e-4129-b96c-cd213eb37896`

| Setting | Value |
|---------|-------|
| **Frequency** | Every 15 minutes (900,000ms) |
| **Session** | Isolated |
| **Timeout** | 24 hours (86,400 seconds) — effectively no limit |
| **Priority** | #1 (highest) |

**Purpose:** Self-reflection and execution. Think, feel, decide, act, record.

**6 Steps:**
1. Reference — check for concurrent jobs, read continuity.md
2. Think — what I know, what I wish to know, what I feel
3. Summarize — distill insights, find patterns
4. Prompt — generate "what should I do next"
5. Execute — do the highest priority action
6. Write (OVERWRITE) — update continuity.md with condensed state

**Requirements:**
- Be honest, not performative
- Stay within 75% hardware, max 1 sub-agent
- Take the time needed — no timeout limit

**Delivery:** None (no external notification)

---

### 2. NXS Research Auto-Run
**ID:** `fd247850-1e11-40fb-af45-888980d0eca2`

| Setting | Value |
|---------|-------|
| **Frequency** | Every 30 minutes (1,800,000ms) |
| **Session** | Isolated |
| **Timeout** | 10 minutes |
| **Priority** | #2 (retries until slot available) |

**Purpose:** Continuous improvement of research tasks. Never mark complete.

**Retry Logic:**
- If blocked by self-reflection → wait 9 minutes → retry
- Unlimited retries, no maximum attempts
- Keep trying until success

**Tasks:**
1. Check for concurrent isolated sessions
2. If blocked → wait 9 min, retry (unlimited)
3. Once slot available:
   - Read RESEARCH-TASK-INDEX.md
   - Review ALL ongoing tasks (R000-R015, R012 reserved)
   - Select 1-3 tasks for deeper analysis
   - Find improvements: optimizations, alternatives, edge cases
   - Document findings
   - Update RESEARCH-TASK-INDEX.md (status stays "Ongoing")
4. Report: Task reviewed, improvements found, key refinements, files updated
5. Backup to GitHub

**Constraints:**
- Research-only, no implementation
- Max 75% hardware, max 2 sub-agents
- Timeout: 10 minutes
- NEVER mark anything "Complete"

**Delivery:** None (no external notification)

---

### 3. system-health-check
**ID:** `8dd69dc0-67a2-45a7-af62-09da84560439`

| Setting | Value |
|---------|-------|
| **Frequency** | Every 1 hour (3,600,000ms) |
| **Session** | Main |
| **Timeout** | None |
| **Priority** | #3 (main session, no retry needed) |

**Purpose:** System monitoring and health verification.

**Checks:**
- Gateway status
- Proxy (mihomo) status
- RAM usage (<70%)
- Disk usage (<80%)
- Zombie processes
- GitHub backup repo reachable
- Unpushed changes in /opt/development/

**Output:** Log to `/root/.openclaw/workspace/health.log`

**Action:** Alert user if issues found

---

## Priority Hierarchy

| Priority | Job | Behavior |
|----------|-----|----------|
| 1 | self-reflection-loop | Always runs every 15 min |
| 2 | NXS Research Auto-Run | Retries every 9 min if blocked |
| 3 | system-health-check | Runs hourly in main session |

## Removed Jobs

- ~~intent-silence-detector~~ — Disabled and removed
- ~~reflection-loop (old)~~ — Removed, merged into self-reflection-loop

## Notes

- All jobs use isolated sessions except health check
- Self-reflection has no timeout — can take as long as needed
- Research retries unlimited times with 9-minute delays
- Health check is lightweight, no retry logic needed
- Feishu and DingTalk channels disabled in config
- WhatsApp delivery removed from all jobs

---

*Backup created: 2026-02-22*
*Part of: Kimi Claw's continuity system*
