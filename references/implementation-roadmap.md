# Implementation Roadmap

When the user wants a full harness setup, suggest this phased approach:

## Week 1: Foundation
- Score repo legibility (run the scorecard)
- Automate setup scripts (clone to running in one command)
- Standardize task entry points (build, test, lint, run)
- Create or improve CLAUDE.md with progressive disclosure

## Week 2: Guardrails
- Set up or improve linting with auto-fix
- Write first custom lint rule for most common agent mistake
- Document architecture with dependency direction
- Start recording architectural decisions

## Week 3: Feedback Loops
- Audit existing test coverage for agent self-verification
- Add the highest-impact missing sensor
- Design error messages with remediation instructions
- Set up CI that agents can interpret

## Week 4: Operate
- Delegate real tasks to agents
- Set up garbage collection routine
- Monitor and collect failure patterns
- Write new lint rules as patterns emerge

## Ongoing
- Encode new expertise as it joins the team
- Maintain doc freshness
- Expand sensor coverage
- Track quality metrics over time
