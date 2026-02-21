# R015 - Non-LLM Mundane Task Scripts

## Status: Planned
## Priority: Medium
## Created: 2026-02-22

## Objective

Research and design self-contained scripts for mundane tasks that don't require LLM function calls. These scripts should be lightweight, reliable, and use libraries already present in the system.

## Motivation

- Reduce unnecessary LLM API calls for simple operations
- Improve response time for routine tasks
- Lower resource consumption
- Enable future simpler/more mundane agent implementations
- Create reusable automation building blocks

## Research Questions

### 1. Task Identification
What qualifies as "mundane"?
- File operations (rotation, cleanup, sync)
- System monitoring (health checks, resource tracking)
- Data transformations (JSON/YAML/CSV conversion)
- API polling (simple GET/POST without reasoning)
- Log aggregation and analysis
- Backup verification
- Simple notifications

### 2. Library Inventory
What libraries are already available?
- Node.js built-ins (fs, path, http, child_process)
- OpenClaw dependencies
- System tools (curl, jq, awk)
- Existing npm packages in the environment

### 3. Architecture Options
- Single standalone scripts (one per task)
- Unified framework with plugin system
- Shell scripts vs Node.js scripts vs Python
- Configuration-driven vs hardcoded

### 4. Integration Points
- How are these triggered? (cron, file watchers, HTTP)
- How do they report results? (logs, files, API calls)
- How do they chain together? (pipelines, dependencies)

### 5. Examples to Research
| Task | Current LLM Approach | Proposed Script Approach |
|------|---------------------|-------------------------|
| Health check | Query via tool calls | Direct system calls script |
| Log rotation | LLM decides when | Size/time-based script |
| Backup verify | LLM interprets git status | Git API script |
| File cleanup | LLM analyzes disk | Age/size-based script |

## Deliverables

1. Inventory of available libraries/tools
2. Categorized list of mundane tasks suitable for scripting
3. Recommended architecture (single scripts vs framework)
4. Example implementations for 3-5 common tasks
5. Integration guide for existing systems

## Constraints

- Use only existing libraries (no new dependencies)
- Scripts must be self-contained
- No LLM calls within scripts
- Must handle errors gracefully
- Must log appropriately

## Notes

- This research is preparation for future simpler agent implementations
- Scripts should be composable (output of one = input of another)
- Consider POSIX compatibility for portability
- Document performance characteristics

---

*Documented: 2026-02-22*
*Requested by: Sepia Quinterra Manju (Architect)*
