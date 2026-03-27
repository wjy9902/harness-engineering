# Scheduled Agent Automations

Don't rely on humans to remember cleanup. Set up recurring agent tasks that run
automatically:

- **Daily**: PR review bot that checks open PRs for pattern violations and leaves comments
- **Daily**: Conflict resolution -- detect and fix merge conflicts across worktrees
- **Weekly**: Slop scanner -- find duplicate functions, dead code, inconsistent patterns;
  open targeted cleanup PRs (each should be reviewable in under 1 minute)
- **Weekly**: Doc freshness audit -- check if docs reference renamed files, outdated
  commands, or removed modules
- **Per-commit**: Quality grades per module, tracked over time in a dashboard or log

OpenAI found these automations essential once velocity exceeded what humans could
manually review. The key: each automation should produce a small, focused PR -- not a
massive cleanup dump.

## Getting Started

When scaffolding, help the user set up at least one scheduled automation (typically the
slop scanner) using cron, GitHub Actions, or Claude Code's `/schedule` feature.
