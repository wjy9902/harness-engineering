# Feature List Template (Long-Task Handoff)

Use JSON because it resists casual modification better than markdown:

```json
{
  "features": [
    {
      "id": "auth-login",
      "category": "functional",
      "description": "User can log in with email and password",
      "verification_steps": [
        "Navigate to /login",
        "Enter valid credentials",
        "Verify redirect to dashboard",
        "Check session cookie is set"
      ],
      "passes": false
    }
  ]
}
```

## Rules for the Coding Agent

- Never delete features from the list
- Never edit feature descriptions
- Only flip `passes` to `true` after end-to-end verification
- One feature per session

## Companion Files

Also create:

- **progress.txt** — Free-form log updated each session: what was done, what's next, known issues
- **Session startup ritual** (add to CLAUDE.md):
  1. Read progress.txt
  2. Read `git log --oneline -20`
  3. Run tests for the target area (baseline)
  4. Read features.json to find the next un-done feature
  5. Work on ONE feature
  6. Run tests (verify)
  7. Commit with descriptive message
  8. Update progress.txt
  9. Update features.json (flip `passes` to `true`)
