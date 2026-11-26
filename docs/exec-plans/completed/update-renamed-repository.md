# Update the public install command after the repository rename

Maintain this ExecPlan according to `PLANS.md`.

## Purpose / Big Picture

Readers should be able to use the current GitHub repository name from `README.md` to find and install the `tilda` skill without relying on a redirect.

## Progress

- [x] (2026-11-26 06:36Z) Confirmed the public repository is `Dmitry-Filippov/tilda-skill` on `main` and identified the stale install command.
- [x] (2026-11-26 06:36Z) Updated the command and verified that the new source lists `tilda`.
- [x] (2026-11-26 06:37Z) Completed final review and recorded the validation outcome.

## Surprises & Discoveries

The new Skills.sh card opens, but the owner-filtered search still returned the previous repository name during the initial check. The search entry is separate from whether the installation source works.

## Decision Log

- Decision: Update only the public command; retain prior test records and leave the local Git remote for the owner. Rationale: The skill did not move within the repository, completed plans record what was tested at the time, and Git is read-only for agents. Date/Author: 2026-11-26 / OpenCode.

## Outcomes & Retrospective

The README now points to the renamed repository. `git diff --check` passed, and the Skills CLI found `tilda` at the new source without installing it. Final review raised one optional documentation finding, accepted here to record the completed validation; no README changes were needed after review. A full install was not tested.

## Context and Orientation

`README.md` is the public installation guide. `tilda/SKILL.md` is the package. The GitHub repository dropped its leading hyphen; the package is still named `tilda`.

## Plan of Work

In the README's Install section, replace `Dmitry-Filippov/-tilda-skill` with `Dmitry-Filippov/tilda-skill` in the CLI command. Check the new source with the CLI's list-only mode, then submit the documentation change for review.

## Concrete Steps

From the repository root, run `git diff --check` and `git diff -- README.md`. With the cached Skills CLI already available in this workspace, run:

    DISABLE_TELEMETRY=1 node "$HOME/.npm/_npx/ac0ed6aa23b37c1e/node_modules/skills/bin/cli.mjs" add Dmitry-Filippov/tilda-skill --list

## Validation and Acceptance

The diff shows the new GitHub source and the unchanged `--skill tilda` selector. The list-only command reports `tilda` without installing it. The status of Skills.sh search is not an acceptance criterion.

## Idempotence and Recovery

Do not duplicate the command if it is already correct. The checks can be repeated without installing the skill or modifying the checkout.

## Artifacts and Notes

Suggested commit: `docs: Update install command after repository rename`.

`git diff --check` passed. List-only output: `Source: https://github.com/Dmitry-Filippov/tilda-skill.git`, `Found 1 skill`, `tilda`.

Revision note: Recorded the successful list-only check and final review outcome.

## Interfaces and Dependencies

No changes to the skill package, Git configuration, or tool interfaces. The list check uses the existing local Skills CLI.
