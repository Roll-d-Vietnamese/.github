<!-- SYNCED from rolld-agent-framework — edit source, not this file. Re-sync with /sync-config. -->

# Contributing

Commit, branch, PR, and merge conventions for all rolld projects.

## Commit Messages

**Format:** `<type>: <short summary>`

- Imperative mood ("add", not "added" or "adds")
- Under 72 characters
- Focus on why, not what

**Types:**

| Type | Purpose |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `infra` | Terraform / AWS changes |
| `docs` | Documentation only |
| `chore` | Tooling, config, dependencies |
| `refactor` | Code restructuring (no behaviour change) |
| `ci` | CI workflow changes |
| `transform` | dbt model changes |
| `test` | Adding or updating tests |
| `revert` | Reverting a previous commit |

**Examples:**
```
feat: add weekly scorecard query for Superset
fix: handle null timezone in shift parser
infra: switch to per-project IAM user
docs: add ADR 015 for PostgreSQL serving layer
transform: add staging model for invoice hourly
```

## Branch Naming

**Format:** `<type>/<short-kebab-description>`

Use the same types as commit messages. Always use short form (never `feature/`).

**Examples:**
```
feat/postgresql-serving-layer
fix/s3-list-cost
infra/per-project-iam
docs/adr-015
```

## Pull Requests

- **Title:** Same format as commit messages, under 70 characters
- **Body:** Must follow the PR template (`.github/pull_request_template.md`):
  - Context (why this PR exists, linked issues/ADRs)
  - Changes (files grouped by category)
  - Test plan (specific verification steps)
  - Review notes (trade-offs, risks, follow-ups)
- **Review:** Minimum 1 review before merge

## Merge Strategy

- **Squash merge only** -- keeps `main` linear with one commit per PR
- PR title becomes the squash commit message
- Feature branch is deleted after merge (remote and local)
