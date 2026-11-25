# Agent instructions

This project develops a reusable Tilda skill. Edit the canonical source at [`tilda/SKILL.md`](tilda/SKILL.md); project initialization does not authorize connecting to or changing a Tilda site.

<!-- init-agent:docs -->
## Project documentation

- [Project map](docs/project-map.md)
- [Design and architecture](docs/design.md)
- [Project conventions](docs/conventions.md)
- [Testing methodology](docs/tests.md)
- [Product](docs/product.md)
- [ExecPlans index](docs/exec-plans/README.md)

Read [the project map](docs/project-map.md) before broad repository searches; use its routing table to start in the relevant area and expand if incomplete or stale. Update routes, tree, and descriptions when structure, responsibilities, entrypoints, or test locations change.

Documentation written by agents is in English by default. If all existing documentation is in one other language, match it. If documentation is mixed, write new files in English and edit existing files in their own language.
<!-- /init-agent:docs -->

## Unexpected obstacles

When an unexpected obstacle arises or the agreed approach needs to change, pause the affected work. Explain the problem and its impact, offer possible actions and a recommendation, and ask the user to decide. Do not apply a workaround or change the approach before their decision. Resolve routine development and test failures independently within the agreed task.

## Testing policy

Use the smallest sufficient verification: existing unit/integration/API tooling and applicable type/lint checks first, then targeted static and scenario checks from [the testing methodology](docs/tests.md). Browser work requires a real-browser or visual scenario, necessary screenshot, or targeted diagnostic. Read the [browser procedure](.opencode/init-agent-files/playwright-chromium.md) and any project browser-route record first. Honor the recorded route; a verified host takes priority over stale sandbox instructions, and permanent fallback persists until the user changes it. Ask to reconcile conflicting instructions. Without a record, prefer Docker MCP Gateway and an existing host browser. An application assertion failure does not mean the host route is unavailable.

Ask before package, browser, or system-library installation or browser startup. List the changes; if declined, report what is blocked or unverified. Preserve host settings and user tabs; use headless Chromium for an approved task-owned sandbox fallback. Browser tools share one budget: one worker/pass, normally one task-created active page, one affected scenario (usually 1–5 tests), 30 seconds per test, 120 seconds total including diagnostics and teardown, and no automatic retries. Report preparation/startup separately with finite deadlines. No watch/UI mode, video, trace, or automated screenshots by default. Wait for explicit readiness, not unconditional `networkidle`. Rerun only the affected check within the remaining time. Full E2E/matrices require a request or project requirement; surface conflicts first. Bound all runner, process, and MCP calls and close only resources created for the task. An MCP timeout is not teardown. The same limits apply to skill examples.

## Optional verification (QA and security)

After implementation and before final review, use [verification-consensus](.agents/skills/verification-consensus/SKILL.md) for applicable work. Delegate scoped checks, browser scenarios, and diagnostics to `qa`; delegate defensive review of sensitive changes or an explicitly requested full audit to `security`. If both apply, run QA first and security second. Documentation-only work does not automatically trigger either. Keep the shared testing/browser budget; do not duplicate delegated checks. Do not infer permission for live or remote targets, installations, or active testing. Follow the skill's consent, isolation, findings, and consensus gates. If a required check cannot run, ask the user to prepare it, narrow it, or explicitly finish with limitations; do not invoke unavailable agents or silently bypass requirements.

## Task review (consensus)

When an implementation task is complete and verified, run [review-consensus](.agents/skills/review-consensus/SKILL.md). Spawn the [read-only reviewer](.opencode/agents/reviewer.md) with a clean context: task, plan path if any, changed files, and validation evidence. For a project without Git, supply the actual changed paths and readable evidence instead of Git output, and explicitly tell the reviewer to skip Git commands and read the listed files and supplied materials directly. Address findings with fixes or concrete counterarguments, for up to two rounds. Apply agreed fixes, rerun affected validation, and escalate unresolved disputes to the user with options.

## ExecPlans (implementation planning)

For tasks that change code or files, check the [ExecPlans index](docs/exec-plans/README.md) and [backlog](docs/exec-plans/backlog.md) before writing. If the task pursues the same user-visible goal as an active plan, append milestones there, update its Purpose if needed, record the decision, and keep Progress current. Ask if the match is ambiguous. Promote a matching backlog entry by marking it `[x]` and linking the plan. If the work overlaps the files of a different inactive-in-practice plan, record its deferral in the Decision Log and add a backlog reminder.

Otherwise read [PLANS.md](PLANS.md) in full and write an ExecPlan at `docs/exec-plans/active/<kebab-case-name>.md`. Clarify ambiguities before writing the plan. Use self-contained, independently verifiable milestones and implement them in order without waiting for approval between milestones. Keep Progress current at every stopping point and note out-of-scope work in the backlog.

After all milestones and validation pass, follow the project's verification and final review rules. Move the plan to `docs/exec-plans/completed/` and update the index only after the required review reaches a terminal state: agreed fixes validated and disputes resolved. If the user explicitly skips review or the skill's empty-reviewer fallback applies, record the reason in Outcomes & Retrospective.
