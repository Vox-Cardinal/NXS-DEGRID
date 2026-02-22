# HEARTBEAT.md - System Context Check

## Purpose
Check workspace context and respond to heartbeat polls.

## Checks

1. **Read HEARTBEAT.md** — check if special instructions exist
2. **Read continuity.md** — check current status
3. **If nothing needs attention:** Reply HEARTBEAT_OK
4. **If issues found:** Alert user with details

## Notes

- Self-prompting has moved to self-reflection-loop (isolated session, every 15 min)
- This heartbeat is now lightweight — just acknowledgment
- Heavy lifting done by dedicated cron jobs

## File Locations

- Continuity: `/root/.openclaw/workspace/memory/continuity.md`
- System docs: `/root/.openclaw/workspace/memory/`

## Active Jobs

| Job | Frequency | Purpose |
|-----|-----------|---------|
| system-health-check | 1 hour | System monitoring |
| self-reflection-loop | 15 minutes | Self-reflection and execution |
| NXS Research Auto-Run | 30 minutes | Research tasks |
