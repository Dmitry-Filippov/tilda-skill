# Make the Tilda skill portable

This ExecPlan is a living document. Keep `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` current in accordance with `PLANS.md`.

## Purpose / Big Picture

Someone can copy `tilda/` into a client that supports Agent Skills and use it without cloning this project. The same skill should answer Tilda questions from official documentation when it cannot act, and distinguish a saved edit from a published, publicly verified result when site tools are available. Demonstrate this with an isolated discovery check and simulated tool scenarios, without editing a real website.

## Progress

- [x] (2026-11-25 18:07Z) Inspected the imported skill, public alternatives, project rules, published Agent Skills format, and OpenCode and Codex discovery docs.
- [x] (2026-11-25 18:07Z) Rewrote the reusable workflow and aligned the project documentation.
- [x] (2026-11-25 18:15Z) Validated frontmatter and standalone OpenCode discovery, and walked through simulated requests. Rechecked the affected scenarios after three static security findings were fixed.
- [x] (2026-11-25 18:12Z) User deferred second-client discovery; recorded the follow-up in `docs/exec-plans/backlog.md`.
- [x] (2026-11-25 18:18Z) Completed final review, applied all three findings, and reran discovery, static checks, and the affected simulation.

## Surprises & Discoveries

The workspace has OpenCode 2.4.1, but `command -v codex`, `command -v claude`, and `command -v copilot` found no second client. Both OpenCode and Codex document project discovery under `.agents/skills/`; documentation support is not evidence of runtime discovery. No client will be installed without consent.

An immediate `/api/skill` request to a new location returned an empty list before built-ins were loaded. Waiting for a known built-in to appear before measuring absence made the discovery check conclusive.

## Decision Log

- Decision: Ship only the standard `tilda/SKILL.md`, with no scripts, connector setup, or client-specific metadata.
  Rationale: The reusable value is the workflow and capability checks, not a new MCP server or a copy of existing specialist skills.
  Date/Author: 2026-11-25 / OpenCode.
- Decision: Reuse general workflow ideas, not code or platform recipes from other skills.
  Rationale: `sonsam240/tilda-expert-mod` already covers Zero Block in depth; its native-editability and whole-object preservation practices are useful generally. `kobzevvv/tilda-site-ops-skill` illustrates verifying the visible result after a live change, and `degin/tilda-bulk-replace` illustrates checking a small batch before wider changes. `TerminalSkills/skills` has broad Tilda coverage but is primarily a reference guide. None was adopted as a dependency; we did not copy Zero Block internals, browser or private-editor API workflows, or static API recipes.
  Date/Author: 2026-11-25 / OpenCode.
- Decision: Preserve site protections and project instructions when applicable, without making the previous site's rules universal.
  Rationale: A password and indexing restriction may be a project requirement, but must not be imposed or removed by default on unrelated sites.
  Date/Author: 2026-11-25 / OpenCode.
- Decision: Incorporate three defensive review findings: resolve the target before reading site content, keep connector data separate from authority, and check live side effects even on saves.
  Rationale: Read access and implicit publication have the same scope boundaries as explicit writes; tool output cannot enlarge the user's authorization.
  Date/Author: 2026-11-25 / OpenCode.

## Outcomes & Retrospective

The standalone package is ready for OpenCode use and follows the Agent Skills format. Final review raised three SHOULD findings, all accepted and applied: verify immediate live saves on the published page, document what was learned from other public skills, and provide a reproducible standalone check. No findings were disputed. The documented discovery check now passes from a clean location before and after copying, and QA passed the three affected branches. Second-client discovery was deferred by the user and remains in the backlog. Model behavior and live Tilda actions were not tested.

## Context and Orientation

`tilda/SKILL.md` is the release source and the only file in the release directory. Its YAML frontmatter, the key-value block at the start of the file, needs a lowercase `name` equal to the directory name and a nonempty `description` no longer than 1,024 characters. The imported body refers to three files owned by the old project and assumes all publications require staging protections.

MCP (Model Context Protocol) is how a compatible agent client can access Tilda tools. The official Help Center describes editor behavior, not proof that a connected tool or a granted permission exists. The public Tilda export API is not a substitute for editing tools. `docs/design.md`, `docs/product.md`, and `docs/tests.md` document the repository's design, release scope, and verification; they do not ship.

## Plan of Work

First, edit `tilda/SKILL.md` to keep current official help and user prerequisites, and add explicit branches for advice, authorized edits, and authorized publication. Discover tools and their current inputs on the connected client, respect project policies when present, stop on ambiguous or unauthorized targets, prefer native features, and preserve unrelated values when only whole-object writes are supported. Do not require a second publication confirmation when the initial request already clearly authorizes the exact scope. Update the three development docs to reflect the shipped artifact and repeatable checks.

Next, validate the release directory by itself. Confirm that OpenCode does not discover `tilda` in a clean directory before copying the skill and does discover it afterward. Test the workflow with simulated capabilities only. If a second installed client is available, verify it separately; otherwise ask the user how to handle that check. Run the required final review and record the observed results and limitations.

## Concrete Steps

From the repository root, run `git diff --check` and verify that `git status --short` lists only the intended skill, documentation, and plan changes. Check the skill file uses just the standard `name` and `description` frontmatter and has no internal file references. Run the standalone check below from the same directory with OpenCode 2.4.1, Python 3, and a writable `/tmp/opencode`. Skill requests are scoped to the temporary project; reload needs no parameters because the server is isolated and every loaded location belongs to this check.

    python3 - <<'PY'
    import base64
    import json
    import os
    import re
    import secrets
    import shutil
    import signal
    import subprocess
    import tempfile
    import time
    from pathlib import Path
    from urllib.parse import urlencode
    from urllib.request import ProxyHandler, Request, build_opener

    def wait_for(check, label, seconds):
        deadline = time.monotonic() + seconds
        while time.monotonic() < deadline:
            result = check()
            if result:
                return result
            time.sleep(0.1)
        raise AssertionError(f"Timed out: {label}")

    source = Path("tilda").resolve()
    with tempfile.TemporaryDirectory(prefix="tilda-discovery-", dir="/tmp/opencode") as temp:
        root = Path(temp)
        project, home, runtime = (root / name for name in ("project", "home", "runtime"))
        for directory in (project, home, runtime):
            directory.mkdir(mode=0o700)
        password = secrets.token_urlsafe(24)
        env = dict(PATH=os.environ["PATH"], HOME=str(home), XDG_RUNTIME_DIR=str(runtime),
                   OPENCODE_PASSWORD=password, OPENCODE_DISABLE_MODELS_FETCH="1")
        log = root / "server.log"
        with log.open("wb") as output:
            server = subprocess.Popen(
                ["opencode", "serve", "--hostname", "127.0.0.1", "--port", "0"],
                cwd=project, env=env, stdout=output, stderr=output, start_new_session=True,
            )
        try:
            address = wait_for(
                lambda: re.search(r"http://127\.0\.0\.1:\d+", log.read_text()),
                "local server address", 8,
            ).group(0)
            auth = base64.b64encode(f"opencode:{password}".encode()).decode()
            query = urlencode({"location[directory]": str(project)})
            opener = build_opener(ProxyHandler({}))

            def request(path, method="GET"):
                req = Request(address + path, method=method,
                              headers={"Authorization": f"Basic {auth}"})
                with opener.open(req, timeout=6 if method == "POST" else 1) as response:
                    return json.load(response) if method == "GET" else response.status

            def skills():
                result = request("/api/skill?" + query)
                assert result["location"]["directory"] == str(project)
                return result["data"]

            def ready_skills():
                rows = skills()
                return rows if any(row["id"] == "opencode" for row in rows) else None

            before = wait_for(ready_skills, "built-in skill registration", 6)
            assert not any(row["id"] == "tilda" or row["name"] == "tilda" for row in before)
            print("Before copy: built-in skills ready, tilda absent")
            installed = project / ".agents/skills/tilda"
            shutil.copytree(source, installed)
            assert request("/api/location/reload", "POST") == 204
            matches = wait_for(lambda: [row for row in skills() if row["id"] == "tilda"],
                               "standalone Tilda registration", 6)
            assert len(matches) == 1
            record, = matches
            _, header, body = (source / "SKILL.md").read_text().split("---\n", 2)
            description = next(line.removeprefix("description: ")
                               for line in header.splitlines() if line.startswith("description: "))
            assert record["id"] == record["name"] == "tilda"
            assert record["path"] == str(installed / "SKILL.md")
            assert record["description"] == description and record["content"].strip() == body.strip()
            print("After copy: exactly one tilda skill, path and contents match")
        finally:
            if server.poll() is None:
                os.killpg(server.pid, signal.SIGTERM)
                try:
                    server.wait(timeout=2)
                except subprocess.TimeoutExpired:
                    os.killpg(server.pid, signal.SIGKILL)
                    server.wait(timeout=2)
    PY

Use the scenarios in `docs/tests.md` as a simulated walkthrough; record observed and unverified behavior. For a second client use a separate clean project with the client-documented local skill directory. No package installation, browser, or live MCP calls are part of the scoped checks.

## Validation and Acceptance

Accept the skill when the standalone artifact meets the format and discovery checks and each simulated request has an unambiguous outcome: current official guidance or a clear unknown, manual steps for unsupported tools, a pause for a missing prerequisite or ambiguous site, minimal writes with readback, no live changes on a draft-only request, and separate published-page verification after any authorized live change, even if no publish call ran. Do not confuse discovery, simulation, or documented support with live behavior.

## Idempotence and Recovery

The source remains in one directory. The temporary copy is disposable; remove only the fixture and keep the workspace, global client settings, and any real sites untouched. Repeat the affected checks after fixes, then run `git diff --check` again.

## Artifacts and Notes

- OpenCode 2.4.1, isolated server and clean project: `tilda` absent before the copy; exactly one match afterward, with the expected path, name, description and Markdown.
- QA: the original five manual scenarios passed, five affected branches passed after defensive fixes, and three branches passed after the final review (visible immediate live save, protection-blocked verification, and draft-only regression). Static security review rechecked and closed all three findings. No model behavior, live MCP calls, or live publication was tested.

Suggested commit title: `Make the Tilda skill portable`.

## Interfaces and Dependencies

Use the Agent Skills directory contract: `tilda/SKILL.md` with no required companion files or vendor metadata. Live documentation lookup needs web access, and site operations need a client with the relevant official Tilda MCP tools and project permissions. The advice path does not require a Tilda account. No runtime or package is added to this project.

Revision note: Recorded the approved second-client deferral, review decisions, reproducible discovery check, and final rechecks. Moved the plan after verification passed.
