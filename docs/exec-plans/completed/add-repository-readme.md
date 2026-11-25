# Add a public README for the Tilda skill

This ExecPlan is a living document maintained under `PLANS.md`.

## Purpose / Big Picture

A visitor to the public GitHub repository can tell what the Tilda skill does, copy an install command for the right repository, and try a question without connecting their site.

## Progress

- [x] (2026-11-25 18:48Z) Confirmed the public repository name and agreed on a short English README.
- [x] (2026-11-25 18:48Z) Added the README and linked it from the project map.
- [x] (2026-11-25 18:49Z) Checked links and installer discovery without installing the skill.
- [x] (2026-11-25) Completed review and accepted all three findings.

## Surprises & Discoveries

The public repository is `Dmitry-Filippov/-tilda-skill`, with a leading hyphen; its default branch is `main`. The local checkout is on `dev`.

## Decision Log

- Decision: Document the existing GitHub name and keep this a documentation-only change.
  Rationale: The install command must use the actual public source. Renaming the repository, pushing, adding a license, and claiming a skills.sh listing are outside scope.
  Date/Author: 2026-11-25 / OpenCode.

## Outcomes & Retrospective

The README documents installation and first use without changing the skill. Final review raised two SHOULD findings and one COULD finding, all accepted: give a concrete manual-install path, explain immediate live effects on a draft-only request, and verify the install flag without installing. No findings were disputed.

## Context and Orientation

`tilda/SKILL.md` is the installable skill. The root `README.md` is the public introduction, and `docs/project-map.md` helps contributors find the relevant file. MCP is the connection agents use to access supported Tilda tools; it is unnecessary for advice.

## Plan of Work

Write the English `README.md` with a one-sentence overview, an install command and a manual alternative, three example prompts, and a short explanation of Tilda MCP and authorization for live changes. Add the README to the project map. Do not change the skill.

## Concrete Steps

From the repository root run `git diff --check` and `git status --short --untracked-files=all`. Verify that the README's local path exists and its two external links resolve. If the `skills` CLI is already available, run its list-only check for the public source (`skills add Dmitry-Filippov/-tilda-skill --list` with telemetry disabled) and expect `tilda` in the result. Do not install anything for this check.

## Validation and Acceptance

The public source can be queried by the documented owner and repository name. The README leads readers to the installable directory, offers an advice-only example, and differentiates a draft edit from a live change. The skill source stays untouched.

## Idempotence and Recovery

Link checks and listing can be repeated without installing the skill or changing the repository on GitHub. There are no migrations or site changes.

## Artifacts and Notes

The cached `skills` CLI (v1.9.0) was run with `DISABLE_TELEMETRY=1 DO_NOT_TRACK=1 GIT_TERMINAL_PROMPT=0` using these arguments:

    skills add Dmitry-Filippov/-tilda-skill --list
    skills add --help

The first reported `Found 1 skill` and listed `tilda`. The second documented `--skill <skills>` and confirmed that `--list` does not install. Both external README links returned HTTP 200 and the local documentation checks passed. The install itself was not run.

Suggested commit title: `docs: Add a public README`.

## Interfaces and Dependencies

The README depends on the existing `tilda/` directory, the public GitHub repository, and the linked official MCP instructions. No project dependency is added.

Revision note: Recorded the edits, review outcomes, and link and list-only checks.
