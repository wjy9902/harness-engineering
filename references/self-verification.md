# Agent Self-Verification Loop (The "Ralph Wiggum" Pattern)

The most powerful feedback loop is when the agent can verify its own work end-to-end
without human help. OpenAI's agents run single tasks for up to 6 hours, unattended,
using this pattern:

```
1. Agent makes a change
2. Agent launches an isolated app instance (per git worktree)
3. Agent runs the app, captures DOM snapshots / screenshots
4. Agent compares actual state to expected state
5. If wrong: fix and go back to step 1
6. If right: commit and move on
```

To enable this, the agent needs access to:

- **App runtime**: ability to start/stop the dev server
- **Browser automation**: Playwright or Puppeteer to interact with UI like a real user
- **Observability tools**: log queries (LogQL/grep), metric queries, error traces
- **Screenshot capture**: for visual regression detection

The key insight: replace subjective "does this look right?" with mechanical "does this
pass?" An agent that can self-verify will iterate until correct. An agent that can't
will hand you half-finished work and say "looks good to me."

## Scaffolding Steps

When setting up self-verification for a project:

1. Ensure `npm run dev` (or equivalent) works headlessly
2. Install Playwright with a basic smoke test
3. Add observability commands to CLAUDE.md so the agent knows how to check logs
4. Document how to run the full verification suite in one command
