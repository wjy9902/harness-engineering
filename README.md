# Harness Engineering

A Claude Code skill that audits your repository's agent-readiness and **builds the missing infrastructure** — CLAUDE.md, architecture docs, custom lint rules, guard tests, feedback loops, and more.

Most AI coding tools advise. This one builds.

## What It Does

When you tell Claude "set up harness for this project" or "the agent keeps drifting", this skill:

1. **Scores** your repo against a 7-dimension Legibility Scorecard (bootstrap, task entry points, validation, linting, codebase map, docs, decision records)
2. **Diagnoses** which part of the control loop is missing (goal, sensor, actuator, or feedback)
3. **Builds** the actual files — tailored to your tech stack, not generic templates

### Files It Creates

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Agent instruction file (~60-100 lines, table of contents style) |
| `docs/ARCHITECTURE.md` | Layer diagram, dependency rules, import restrictions |
| `docs/CONVENTIONS.md` | Naming, patterns, anti-patterns |
| Custom lint rules | ESLint/Ruff/golangci-lint rules encoding architecture as errors |
| Architecture guard tests | Tests that enforce import boundaries (AST-based) |
| `scripts/setup.sh` | One-command bootstrap (clone to running) |
| `migration/features.json` | JSON feature list for long-task handoff |
| Pre-commit hooks | Scope guards, pattern enforcement |
| `docs/decisions/` | Architecture Decision Records |

Everything is generated from your actual codebase — real directories, real commands, real patterns.

## Core Idea

**Harness Engineering = Cybernetics for AI-Assisted Development**

```
Goal State + Sensor + Actuator + Feedback Loop = Control System
```

- Without a goal, the agent generates blindly
- Without sensors (lint, tests, type checks), it doesn't know it's wrong
- Without actuators, it can't fix what it finds
- Without feedback, it repeats the same mistakes forever

The engineer's role shifts from writing code to designing the control system that lets agents write code correctly.

## Install

```bash
git clone https://github.com/wjy9902/harness-engineering.git ~/.claude/skills/harness-engineering
```

Claude Code will automatically discover the skill.

## Usage

Natural language triggers — no slash command needed:

```
"Audit my repo for agent readiness"
"The agent keeps making the same mistakes"
"Set up harness for this project"
"Why does the agent ignore our conventions?"
"Help with long-running agent tasks"
"Too much AI slop in the codebase"
```

## Benchmark Results

Tested across 8 scenarios, 4 iterations, 3 ecosystems (Python/FastAPI, Go, TypeScript/React, Django/DRF), in both English and Chinese prompts.

| Metric | With Skill | Without Skill |
|--------|-----------|--------------|
| Assertions passed | **51/51 (100%)** | 14/51 (27%) |
| Builds actual files | Yes (11-17 files per invocation) | No (advises only) |
| Avg tokens | ~41K | ~17K |
| Avg duration | ~303s | ~77s |

The gap is purely execution. Without the skill, Claude gives excellent conceptual advice but creates zero files. With it, Claude builds the complete harness infrastructure.

### Test Scenarios

| Scenario | Stack | What It Tests |
|----------|-------|---------------|
| Audit repo slop | TypeScript/React | Full scorecard + scaffold |
| Long-task handoff | Python/FastAPI | Two-agent pattern, feature list, session protocol |
| Architecture drift | TypeScript/Next.js | Custom lint rules, guard tests |
| Feedback loops | Python/FastAPI | Async compliance, architecture enforcement |
| Go monorepo setup | Go (3 services) | depguard, cross-service import guards |
| Garbage collection | TypeScript/Next.js | Dead code detection, knip setup |
| Django migration | Django/DRF | AST-based migration linter, 80-endpoint tracking |
| Agent overreach | TypeScript/React | Scope guards, pre-commit hooks |

## How It Works

The skill follows a three-level progressive disclosure pattern:

```
SKILL.md (468 lines)          — Core instructions, always in context
references/                   — Deep dives, loaded on demand
  ├── scorecard-template.md   — Output format for the 7-dimension audit
  ├── self-verification.md    — Agent self-verification loop pattern
  ├── feature-list-template.md — JSON template for long-task handoff
  ├── scheduled-automations.md — Recurring cleanup agent tasks
  ├── implementation-roadmap.md — 4-week phased rollout plan
  └── mental-models.md        — Analogies for explaining concepts
```

## Key Concepts

- **Enforcement Hierarchy**: Compiler > Linter > CI > CLAUDE.md > Verbal conventions. Push rules as high as possible.
- **Remediation-focused errors**: Lint error messages are agent context. Write them like you're teaching a new team member.
- **Two-Agent Pattern**: Initializer (sets up once) + Coding Agent (runs per session) for tasks spanning multiple context windows.
- **JSON over Markdown**: Feature lists use JSON because it resists casual agent modification.

## Background

Built from insights in:
- [OpenAI's Harness Engineering](https://openai.com/index/managing-ai-agent-development/) — 7-dimension legibility framework, scheduled automations
- [Anthropic's Effective Harnesses](https://www.anthropic.com/engineering/effective-harnesses) — Two-agent pattern, context window management
- George Zhang's cybernetics framing — Goal/Sensor/Actuator/Feedback model

## License

MIT
