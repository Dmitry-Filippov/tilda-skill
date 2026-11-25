# Testing methodology

## How we test

- Use the project's existing unit, integration, API, type, or lint checks first if introduced; choose the smallest sufficient verification. There is no application runner or automated skill suite today.
- Use `tdd` when appropriate to pin behavioral regressions. Retain meaningful scenarios that can be evaluated through the skill's inputs and outputs.
- Check the skill frontmatter, changed links and referenced files, and walk through affected prompts with simulated tool availability before any live test. For installation changes, confirm discovery in the target client separately from file existence.
- Reserve browser work for a necessary real-browser or visual scenario, screenshot, or targeted diagnostic. Use `webapp-testing` only when needed and available. Initialization did not establish a browser route; for a later justified check follow the [browser procedure](../.opencode/init-agent-files/playwright-chromium.md) and any recorded project route. Ask before any tooling installation or browser startup.

## Local browser verification budget

These defaults apply only if a later task genuinely needs a browser. Full E2E, browser/device matrices, or load tests require a request or project requirement; surface any budget conflict first.

- Browser tools share one 120-second pass including diagnosis and teardown: one worker/pass, normally one task-created active page, one affected scenario (usually 1–5 tests), 30 seconds per test, zero automatic retries. Report preparation and startup separately with finite deadlines. Do not run Playwright and MCP browsers concurrently.
- Use the recorded route if one exists. Preserve host settings and user tabs; do not restart a user's browser to force headless. Use headless Chromium only for an approved, task-owned sandbox fallback.
- Reuse a suitable runner or server. No watch/UI mode, video, trace, or automatic screenshots by default. Wait for explicit locator, response, or application readiness, not unconditional `networkidle` or arbitrary sleeps.
- Rerun only an affected check after a fix within the remaining time; switching tools does not reset the budget. Report passed, failed, and unverified behavior when it is exhausted.
- Bound runner, process, and MCP calls. Reserve teardown time; close only task-created pages and stop only task-owned processes, including on failure. An MCP timeout is not teardown.

### Existing Playwright Test projects

This project has no Playwright runner. If a future task uses an existing approved runner, select one affected scenario and the runner's actual Chromium project, with one worker, zero retries, bounded test and global timeouts, and diagnostics off by default. Use the [procedure](../.opencode/init-agent-files/playwright-chromium.md) and its [deadline supervisor](../.opencode/init-agent-files/run-browser-check.py) for sandbox processes; runner flags alone do not bound cleanup. Do not install or switch runners just to run a template example.

## Rules

- Test observable behavior, not exact prose. Distinguish a static review, a simulated tool scenario, and a live verification.
- Cover the cases relevant to the change: uncertain guidance checks official docs; an unavailable MCP capability is reported; a user-owned prerequisite pauses dependent work; saved state is not reported as published.
- Do not test by editing or publishing a live site. Real site operations require explicit permission and an appropriate test target. Redact sensitive output and clean up only task-owned test data.

## Running

For a skill-only change, inspect `tilda/SKILL.md`, validate its YAML frontmatter and referenced paths, and run the smallest applicable scenario from the rules above. No package installation or browser launch is needed for static checks.
