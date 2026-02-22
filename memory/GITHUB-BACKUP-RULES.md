# GitHub Backup Access Rules

## Critical Requirement

When interacting with the GitHub backup repo (Vox-Cardinal/NXS-DEGRID), **ALWAYS** use the GitHub API with authentication token.

## Token Location

`~/.openclaw/credentials/github/token.json`

## Rules

1. **NEVER** use raw `git` commands for backup verification (git status, git fetch, etc.)
2. **ALWAYS** use GitHub API endpoints
3. **ALWAYS** include the token in the Authorization header
4. **Format:** `Authorization: Bearer <token>`

## API Endpoints to Use

- Repo status: `GET https://api.github.com/repos/Vox-Cardinal/NXS-DEGRID`
- Recent commits: `GET https://api.github.com/repos/Vox-Cardinal/NXS-DEGRID/commits`
- Compare local vs remote: Use API to check latest commit SHA

## Why This Matters

- Security: Token is properly scoped and stored
- Reliability: API gives structured responses
- Consistency: All backup operations use same auth method
- Architect's requirement: Must work even when I forget

## Health Check Integration

The system-health-check cron now includes backup verification via GitHub API.

---

*Documented: 2026-02-22*
*Requirement from: Sepia Quinterra Manju (Architect)*
