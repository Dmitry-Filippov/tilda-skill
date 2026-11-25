# Project conventions

## Language and code style

Use English for new project documentation. Follow the existing concise, imperative Markdown style in the skill and keep `name: tilda` and a non-empty, task-oriented `description` in its YAML frontmatter. No project formatter or linter is configured.

## Structure

Edit [`tilda/SKILL.md`](../tilda/SKILL.md) as the canonical release source, not a development copy. Keep any future skill-owned supporting files under `tilda/`; keep project guidance, validation notes, and planning under `docs/`. Name new docs in lowercase kebab-case, apart from conventional entrypoints such as `docs/exec-plans/README.md`.

## Dos and don'ts

- Separate reusable skill instructions from rules that apply to a particular Tilda site or another repository.
- Link to official documentation when giving procedural guidance. Do not guess MCP names, arguments, access, or outcomes.
- Use sanitized examples. Never add actual credentials, target identifiers tied to customers, or exports containing private data.
- Keep the bundled `/init-agent` support files and installed development helpers separate from the Tilda release.
