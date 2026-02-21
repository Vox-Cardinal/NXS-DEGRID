# Integration Patterns

## Pattern 1: Lazy Loading

**Use Case:** Heavy components like browser, voice models

**Flow:**
```
User Request → Check if loaded
    ↓ Not loaded
Download/Install → Load into memory
    ↓ Loaded
Execute Request
```

**Example:** Browser automation
- First request: Download Chromium (if not present), launch, execute
- Subsequent: Use existing instance
- Timeout: Unload after period of inactivity

**Benefits:**
- Low baseline resource usage
- Pay-for-what-you-use
- Faster startup

---

## Pattern 2: Permission-Based Action

**Use Case:** The Doctor system maintenance

**Flow:**
```
Doctor Monitors → Detects Issue
    ↓
Creates Diagnosis → Sends to NXS
    ↓
NXS Reviews → Grants/Denies Permission
    ↓ Granted
Doctor Executes Fix
    ↓
Reports Result
```

**Example:** Disk cleanup
- Doctor: "Disk 85% full, suggest cleaning /tmp"
- NXS: Reviews, approves
- Doctor: Cleans /tmp, reports freed space

**Benefits:**
- Safety - no unauthorized changes
- Transparency - user knows what's happening
- Control - user can deny risky operations

---

## Pattern 3: API Gateway

**Use Case:** Frontend to backend communication

**Flow:**
```
Frontend (Browser) ←→ API Gateway ←→ Services
                          ↓
                    Route to:
                    - NXS Core
                    - Voice (XTTS/Whisper)
                    - Browser
                    - External APIs
```

**Protocols:**
- WebSocket: Real-time chat, voice streaming
- HTTP REST: File operations, configuration
- SSE: Status updates, logs

**Benefits:**
- Single entry point
- Protocol translation
- Authentication/authorization
- Rate limiting

---

## Pattern 4: Modular Extension

**Use Case:** Skills/extensions system

**Flow:**
```
Core NXS ←→ Extension Manager ←→ Extensions
                ↓
            Load/Unload
            Validate
            Isolate
```

**Lifecycle:**
1. Discover - Find extension files
2. Validate - Check compatibility
3. Load - Initialize in isolated context
4. Register - Add hooks/commands
5. Run - Execute as needed
6. Unload - Cleanup when disabled

**Example:** Browser automation skill
- Installed: Files in ~/.openclaw/skills/
- Loaded: On first use or manual enable
- Running: Available for fetch/screenshot
- Unloaded: After timeout or manual disable

**Benefits:**
- Core stays lean
- Extensions can't crash core
- User controls what's active
- Easy to add/remove

---

## Pattern 5: Resource Guard

**Use Case:** Stay within 75% hardware limit

**Flow:**
```
Request → Check Resources
    ↓ Available
Execute
    ↓ Limited
Queue/Defer
    ↓ Critical
Deny + Alert
```

**Checks:**
- RAM: Current + requested < 75% total
- Disk: Current + requested < 75% total
- CPU: Load average < cores * 0.75
- Sub-agents: Count < 2

**Actions:**
- Green: Proceed
- Yellow: Queue, wait for resources
- Red: Deny, suggest alternatives

**Benefits:**
- System stability
- Predictable performance
- Graceful degradation

---

## Pattern 6: Research → Document → Decide

**Use Case:** NXS development workflow

**Flow:**
```
1. Check Requirements ← NXS-DEV-GUIDE.md
    ↓
2. Check Research Log ← /opt/development/memory/*.md
    ↓
3. Formulate Task ← RESEARCH-TASK-INDEX.md
    ↓
4. Execute Research
    ↓
5. Document Findings → /opt/development/memory/*.md
    ↓
6. Update Guides → NXS-DEV-GUIDE.md
    ↓
7. Next Session
```

**Artifacts:**
- Research notes in /opt/development/memory/
- Decisions in DECISION-LOG.md
- Comparisons in TECHNOLOGY-MATRIX.md
- Tasks in RESEARCH-TASK-INDEX.md

**Benefits:**
- Organized knowledge
- Traceable decisions
- Reproducible research
- Clear next steps

---

## See Also
- [Research Process](../NXS-DEV-GUIDE.md#research-process) - Detailed workflow in main guide
- [Research Task Index](../RESEARCH-TASK-INDEX.md) - Current research tasks
- [Technology Matrix](../TECHNOLOGY-MATRIX.md) - Technology comparisons
- [Decision Log](../DECISION-LOG.md) - Architecture decisions

---

*Patterns are living documents - update as system evolves*
