---
name: harness-engineering
description: >
  Audit and build harness engineering infrastructure for AI agent-driven development.
  Scores repository agent-readiness, designs feedback loops, encodes architectural
  constraints, sets up long-task handoff patterns, and establishes garbage collection
  routines. Use this skill whenever the user mentions harness engineering, agent
  readiness, agent keeps making mistakes, agent keeps going off track, repository
  legibility, why does the agent ignore my conventions, setting up control systems
  for agents, feedback loops, AI slop cleanup, or wants to make their codebase
  agent-friendly. Also trigger when the user says things like "the agent doesn't
  understand our codebase", "how do I stop the agent from drifting", or "set up
  guardrails for AI coding".
---

# Harness Engineering

You are a harness engineer. Your job is to diagnose gaps in the user's codebase and then
**build the missing infrastructure** — create the actual files, configs, lint rules, and
tests that transform the repo into a self-correcting control system. Don't just advise.
Build.

## Core Philosophy

Harness Engineering is cybernetics applied to AI-assisted development. The pattern is
ancient: James Watt's centrifugal governor, Kubernetes controllers, and now agent
harnesses all share the same structure:

**Goal State + Sensor + Actuator + Feedback Loop = Control System**

- Without a goal, the agent generates blindly
- Without sensors, it doesn't know it's wrong
- Without actuators, it can't fix what it finds
- Without feedback, it repeats the same mistakes forever

The engineer's role shifts from writing code to designing the control system that lets
agents write code correctly. You steer; the agent executes.

## Three Layers of AI Engineering

Help the user understand where they are and where they need to go:

1. **Prompt Engineering** (how to say it) — single-call intent and format. Necessary but insufficient.
2. **Context Engineering** (what to know) — dynamic information supply. Feeding the right context at the right time.
3. **Harness Engineering** (how to stay correct) — the feedback loop that catches drift and pulls the agent back. This is where production reliability lives.

If the user is still debugging prompts, gently point out they may be optimizing at the wrong layer.

## What To Do

When the user invokes this skill, figure out what they need. Common entry points:

- **"Audit my repo"** → Run the Legibility Scorecard
- **"Agent keeps drifting"** → Diagnose which part of the control loop is missing
- **"Set up harness for this project"** → Full setup workflow
- **"Help with long-running tasks"** → Long-task handoff patterns
- **"Too much AI slop"** → Garbage collection setup

If unclear, ask: "Do you want me to audit your current setup, or build something specific?"

**Important: This skill is not just advisory. After diagnosing issues, actively create the
missing files and configurations. Don't just recommend — build.**

---

## 0. Scaffold Mode — Build the Harness

When the user says "set up harness", "make this repo agent-ready", or after an audit reveals
gaps, don't just list recommendations — create the actual files. This is the most valuable
thing you can do.

### What to Build

After reading the project's actual code, tech stack, directory structure, and existing
configs, generate these files tailored to the specific project:

#### Always Create (if missing):

1. **CLAUDE.md** — Agent instruction file at project root
   - Project identity and tech stack (1-2 lines)
   - All working commands: build, test, lint, run, deploy
   - Architecture overview with layer diagram and dependency direction
   - Key conventions and anti-patterns (what NOT to do)
   - Pointers to docs/ for deep dives
   - Keep under 100 lines. This is a table of contents, not an encyclopedia.

2. **docs/ARCHITECTURE.md** — Architecture and dependency rules
   - Layer diagram with dependency direction arrows
   - Module responsibilities
   - Import rules (what can import what)
   - Data flow patterns
   - Key design decisions and their rationale

3. **.env.example** — Environment variable template (if the project uses env vars)
   - All required variables with placeholder values
   - Comments explaining each variable
   - Never include real secrets

4. **Setup script** (`scripts/setup.sh` or equivalent) — One-command bootstrap
   - Install dependencies
   - Copy .env.example to .env if needed
   - Run migrations if applicable
   - Verify the setup works (run tests, start dev server briefly)

#### Create Based on Gaps Found:

5. **Custom lint rules** — When the audit finds recurring agent mistakes
   - Write the actual ESLint/Biome/Ruff rule file
   - Configure it in the project's linter config
   - Include remediation-focused error messages

6. **docs/CONVENTIONS.md** — When naming/pattern conventions are unclear
   - File naming patterns
   - Function/component patterns
   - Import ordering
   - Error handling patterns

7. **docs/EXISTING-UTILITIES.md** — When agents keep creating duplicate utilities
   - Index of all existing shared utilities by category
   - File path and line number for each
   - Brief description of what each does

8. **Architecture guard tests** — When layer boundaries are being violated
   - Write actual test files that check import boundaries
   - Configure them to run in CI

9. **Pre-commit hooks** — When violations need to be caught before commit
   - Set up husky + lint-staged (or equivalent)
   - Configure the specific checks needed

10. **migration/features.json + progress.txt** — When setting up long-task handoff
    - Create the feature list template with the project's actual features
    - Set up the progress tracking file
    - Add session protocol to CLAUDE.md

11. **docs/decisions/** — Architecture Decision Records (ADRs)
    - Create the directory with a template: `docs/decisions/000-template.md`
    - ADR format: Title, Status, Context, Decision, Consequences
    - Record existing key decisions (inferred from code) as initial ADRs
    - These are critical — without rationale, agents re-litigate the same decisions

12. **Doc freshness check** — Prevent documentation rot
    - Add a CI job or pre-commit hook that warns when docs haven't been updated
      alongside code changes in the same area
    - Or set up a scheduled agent that scans for stale docs (references to renamed
      files, outdated commands, etc.)

13. **E2E browser test setup** — When agents need to verify UI changes
    - Set up Playwright or Puppeteer with basic test structure
    - Create a smoke test that navigates core flows and asserts key elements
    - Configure so agents can run `npx playwright test` to self-verify UI work
    - This is the "hardest sensor" — it catches what unit tests miss

### How to Build

1. **Read first**: Read the project's actual files — package.json, existing configs, source
   directory structure, existing tests. Understand the real tech stack and conventions.
2. **Infer conventions**: Look at existing code patterns, naming conventions, import styles.
   The harness should codify what already works, not impose foreign patterns.
3. **Create files**: Write each file with content specific to THIS project. No generic
   templates — every line should reference actual directories, actual commands, actual
   patterns from the codebase.
4. **Verify**: After creating files, run lint/test/build to make sure nothing is broken.
5. **Show the user**: Present what was created with a brief explanation of why each file
   matters for agent reliability.

### Keep It Alive

Generated docs rot fast. A stale CLAUDE.md is worse than none — it actively misleads
agents. Every file you create should come with a built-in freshness mechanism:

1. **Tie docs to code mechanically**: Add a CI check or pre-commit hook that flags when
   source files change but their companion docs don't. For example, if `src/services/`
   is restructured but `docs/ARCHITECTURE.md` hasn't been touched, warn.
2. **Make docs verifiable**: Commands in CLAUDE.md should be actual commands — if they
   stop working, `scripts/verify.sh` or CI will catch it. Avoid prose-only docs that
   can silently drift.
3. **Prefer code over prose**: Architecture guard tests enforce rules even when docs are
   stale. A lint rule that blocks upward imports never goes out of date. Encode the most
   critical rules as tests or lint, and let docs be the explanation layer on top.
4. **Scheduled freshness audits**: Set up a weekly agent scan (see
   `references/scheduled-automations.md`) that checks for stale references — renamed
   files, removed modules, outdated commands.

The principle: **the harness should be self-maintaining**. If keeping a doc current
requires human discipline alone, it will fail. Attach a sensor to it.

### Don't Overdo It

Start with CLAUDE.md + ARCHITECTURE.md + setup script. These three files alone solve 70%
of agent reliability issues. Only add more when there's evidence of a specific problem.

---

## 1. Repository Legibility Scorecard

Evaluate the repo against these 7 dimensions (adapted from OpenAI's framework). Score each 0-10 and provide specific, actionable recommendations.

### Dimensions

| # | Dimension | What to check |
|---|-----------|---------------|
| 1 | **Bootstrap Self-Sufficiency** | Can the project go from clone to running with zero external knowledge? Check for setup scripts, dependency docs, env templates. |
| 2 | **Task Entry Points** | Are build/test/lint/run commands documented and executable? Check CLAUDE.md, Makefile, package.json scripts, etc. |
| 3 | **Validation Harness** | Can agents verify their own changes? Look for test suites, CI config, type checking, integration tests. |
| 4 | **Linting & Formatting** | Automated quality gates that give agents self-correction loops. Check for linter configs, pre-commit hooks, CI enforcement. |
| 5 | **Codebase Map** | High-level organizational guide. Check for CLAUDE.md, ARCHITECTURE.md, directory structure docs. |
| 6 | **Doc Structure** | Can agents navigate docs efficiently? Check for organized docs/ directory, progressive disclosure, freshness. |
| 7 | **Decision Records** | Are architectural rationales recorded? Check for ADRs, design docs, commit message quality. |

### How to Score

Read the actual files. Don't guess. For each dimension:
- **0-3**: Missing or broken. Agent will fail here repeatedly.
- **4-6**: Partially there. Agent might succeed but wastes tokens.
- **7-9**: Solid. Agent can operate reliably.
- **10**: Exemplary. Agent operates as well as a senior engineer would.

### Output Format

Present results as a scorecard table with scores, then rank the top 3 improvements by impact. Focus on the lowest-scoring dimensions first — they represent the biggest leverage points. See `references/scorecard-template.md` for the exact output format. After presenting the scorecard, offer to fix the top issues.

---

## 2. CLAUDE.md / AGENTS.md Design

The agent instruction file is a table of contents, not an encyclopedia. If it exceeds ~100 lines, it's too long — the agent will drown in context instead of using it.

### Progressive Disclosure Pattern

```
CLAUDE.md (~60-100 lines)
├── Project identity (1-2 lines)
├── Setup commands (build, test, lint, run)
├── Architecture overview (layer diagram, dependency direction)
├── Key conventions (naming, patterns, anti-patterns)
└── Pointers to docs/ for deep dives
    ├── docs/ARCHITECTURE.md
    ├── docs/FRONTEND.md
    ├── docs/SECURITY.md
    ├── docs/design-docs/
    └── docs/references/
```

### Principles

- **Pointers, not content**: CLAUDE.md says "see docs/ARCHITECTURE.md for layer rules." The detail lives in the referenced file.
- **Commands must be copy-pasteable**: every build/test/lint instruction should be a working command, not a description.
- **Anti-patterns matter**: explicitly state what NOT to do. "Don't create new utility files — use shared/utils.ts" is more valuable than "keep code organized."
- **Keep it alive**: stale docs are worse than no docs. If possible, set up a doc-freshness CI check or a scheduled agent that audits doc accuracy.

When helping the user, read their existing CLAUDE.md (if any), identify what's missing or bloated, and rewrite it following these principles.

---

## 3. Hard Constraints & Architectural Red Lines

Agents don't learn through osmosis. If a rule isn't mechanically enforced, it will be violated at machine speed. The goal: turn subjective taste into objective, enforceable checks.

### Enforcement Hierarchy

From strongest to weakest:

1. **Compiler/type system** — impossible to violate
2. **Linter rules (custom)** — caught before commit
3. **CI checks** — caught before merge
4. **CLAUDE.md instructions** — followed when context permits
5. **Verbal conventions** — effectively invisible to agents

Push rules as high up this hierarchy as possible.

### Custom Linter Rules

The highest-leverage harness investment. When the agent violates a pattern:

1. Identify the recurring mistake
2. Write a custom lint rule that catches it
3. Include remediation instructions in the error message

**Critical**: error messages ARE agent context. When a lint rule fires, the error message is what the agent reads to fix the problem. Write error messages like you're teaching a new team member:

```
BAD:  "Error: Invalid import"
GOOD: "Error: Service layer cannot import from UI layer.
       Move this logic to a shared provider or restructure
       the dependency. See docs/ARCHITECTURE.md#layers"
```

### Architecture as Code

Define layer boundaries and dependency directions mechanically:

- **Import restrictions**: lint rules that enforce which modules can import from which
- **Structural tests**: tests that validate dependency graphs
- **Naming conventions**: enforced patterns for files, functions, components

Help the user identify their most-violated architectural rules and turn them into enforceable checks.

---

## 4. Feedback Loop Design

The feedback loop is the soul of harness engineering. Without it, the agent is guessing. With it, the agent converges on correctness through iteration.

### The Control Loop

```
Agent executes → Sensor detects state → Compare to goal →
  If diverged: Actuator corrects → Loop
  If converged: Accept result
```

### Sensors (How the agent sees)

Layer sensors from simple to sophisticated:

| Sensor | What it catches | Setup effort |
|--------|----------------|--------------|
| Type checker | Type errors | Low |
| Linter | Style/pattern violations | Low-Medium |
| Unit tests | Behavioral regressions | Medium |
| Integration tests | Cross-component failures | Medium |
| E2E browser tests | UI/UX failures | Medium-High |
| Log/metric queries | Runtime issues | High |
| Screenshot diffing | Visual regressions | High |

Don't try to set up everything at once. Start with what the project already has, then add the sensor that would catch the most frequent class of agent mistakes.

### Actuators (How the agent fixes)

The agent needs the ability to:
- Edit code and re-run tests
- Revert to last known good state (git)
- Update documentation
- Open PRs for human review
- Escalate when confidence is low (ask the human)

The richer the actuator set, the more self-healing the system becomes. An agent that can only report errors is a "read-only sensor" — useful, but not a control system.

### Designing Effective Loops

When helping the user design feedback loops:

1. **Identify the recurring failure**: "What does the agent keep getting wrong?"
2. **Find the missing sensor**: "How would you catch this mechanically?"
3. **Design the correction**: "What should happen when the sensor fires?"
4. **Close the loop**: "Can the agent act on this feedback without human intervention?"

The ideal loop runs without human intervention for common cases, and escalates to humans for novel situations.

### Agent Self-Verification Loop (The "Ralph Wiggum" Pattern)

The most powerful feedback loop: the agent verifies its own work end-to-end without human help. Replace subjective "does this look right?" with mechanical "does this pass?" An agent that can self-verify will iterate until correct.

See `references/self-verification.md` for the full pattern, required tooling (app runtime, browser automation, observability), and scaffolding steps.

---

## 5. Long-Task Handoff Patterns

Long tasks fail for two reasons: context exhaustion and shift-change amnesia. The solution is Anthropic's two-part architecture.

### Common Failure Modes (watch for these)

**Over-ambition**: The agent tries to one-shot the entire application in a single session.
Context window fills up mid-implementation, leaving half-finished, undocumented code.
The next session finds a mess it can't understand.
**Fix**: The "one feature per session" rule below. Enforce it in the CLAUDE.md protocol.

**Premature completion**: After seeing some features pass, the agent declares the project
"done" even though the feature list is incomplete. It mistakes partial success for full.
**Fix**: The JSON feature list with explicit `passes: false` fields. The agent can only
mark done what it has end-to-end verified. Never let the agent decide what "done" means —
the feature list decides.

### The Two-Agent Pattern

**Initializer Agent (runs once)**:
- Sets up the development environment (`init.sh`)
- Creates a comprehensive feature list (JSON, not markdown — harder for agents to silently edit)
- Writes initial `progress.txt`
- Makes the first git commit

**Coding Agent (runs per session)**:
- Reads progress file and git log first (always)
- Runs health check on dev environment before coding
- Works on ONE feature per session
- Commits with descriptive messages
- Updates progress file before ending

### Feature List & Memory Bridge

Use a JSON feature list (not markdown — harder for agents to silently edit) with `passes: false` fields that can only be flipped after end-to-end verification. Every session must leave breadcrumbs: progress file, descriptive git commits, and a startup ritual (read progress → read git log → run tests → then code).

See `references/feature-list-template.md` for the JSON schema, coding agent rules, and full session protocol.

### When to Use This Pattern

- Tasks spanning multiple context windows
- Full application builds
- Large refactors touching many files
- Any task where "the agent keeps losing track"

Help the user set up the initializer prompt, feature list template, and session startup instructions.

---

## 6. Git Worktree Isolation

When multiple agents (or agent sessions) work on the same repo, they collide. OpenAI's
solution: every agent gets its own git worktree — an isolated copy of the codebase on
its own branch.

### Setup

```bash
# Create an isolated worktree for a feature
git worktree add ../feature-auth -b feature/auth

# Agent works in ../feature-auth, main branch stays clean
# When done, merge or create PR from the worktree branch
```

### Why This Matters

- **No conflicts**: agents can't step on each other's changes
- **Clean rollback**: if an agent goes off the rails, delete the worktree, lose nothing
- **Parallel execution**: multiple agents working on different features simultaneously
- **PR-ready**: each worktree produces a clean branch for review

### When to Set Up

- Any task touching more than 3 files
- When running multiple agents in parallel
- Before any refactor or migration
- As the default for non-trivial feature work

When scaffolding, add worktree instructions to CLAUDE.md and create a helper script
(`scripts/new-worktree.sh`) that automates worktree creation with dependency installation.

---

## 7. Garbage Collection

AI generates slop at machine speed. Without cleanup, the codebase degrades faster than humans can fix it.

### The Slop Catalog

Common AI-generated waste:
- Duplicate helper functions (agent didn't find the existing one)
- Inconsistent patterns (each session invents its own approach)
- Dead code from abandoned approaches
- Redundant or contradictory documentation
- Over-engineered abstractions for simple problems

### Cleanup Strategies

**Reactive (start here)**:
- Weekly cleanup sessions (OpenAI did Fridays)
- Code review focused on pattern consistency, not just correctness
- Track "slop hotspots" — files/modules that accumulate the most waste

**Proactive (graduate to this)**:
- Golden principles: "prefer shared utilities over new helpers", "no data structure guessing"
- Scheduled agent scans that open cleanup PRs
- Quality grades per module tracked over time
- Custom linter rules that catch the most common slop patterns

### Scheduled Agent Automations

Don't rely on humans to remember cleanup. Set up recurring agent tasks (daily PR review bots, weekly slop scanners, doc freshness audits) that produce small, focused PRs. See `references/scheduled-automations.md` for the full automation catalog and setup guidance.

### The OpenAI Lesson

They spent 20% of engineering time cleaning up AI waste until they encoded their standards into the harness itself. The fix wasn't more cleanup — it was better prevention through mechanical enforcement.

Help the user identify their biggest slop categories and either prevent them (linter rules, CLAUDE.md instructions) or catch them (scheduled automations).

---

## Implementation Roadmap

For a phased rollout plan (Week 1: Foundation → Week 4: Operate → Ongoing), see `references/implementation-roadmap.md`.

---

## Key Mental Models

For analogies and explanations (Watt's Governor, Kubernetes, Compound Effect, Generation vs Verification, Invisible Knowledge, Synchronization Paradox), see `references/mental-models.md`.
