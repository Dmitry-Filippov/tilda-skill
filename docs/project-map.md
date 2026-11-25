# Project map

This project develops and prepares a reusable agent skill for work on Tilda. It does not contain or serve a website.

## Where to start

| Change area | Start here | Relevant tests/checks |
| --- | --- | --- |
| Public overview, installation, or first-use examples | [`README.md`](../README.md) | Check links and confirm that the documented source exposes `tilda` |
| Tilda skill behavior, MCP workflow, or safety instructions | [`tilda/SKILL.md`](../tilda/SKILL.md), then [design](design.md) for the trust boundaries | [Skill checks and scenarios](tests.md) |
| Skill packaging and release scope | [Product](product.md), [conventions](conventions.md), and [`tilda/SKILL.md`](../tilda/SKILL.md) | Review the release contents and validate discovery in the intended client; see [testing](tests.md) |
| Agent development setup or planning | [`AGENTS.md`](../AGENTS.md), [`opencode.json`](../opencode.json), and the [ExecPlans index](exec-plans/README.md) | Validate config syntax, local discovery, and documentation links |

Read this map before broad repository searches. Start in the relevant area;
expand if it is incomplete or stale, then correct the affected route.

## Tree

```text
tilda-skill/
├── README.md                 # Public overview, installation, and examples
├── tilda/SKILL.md            # Canonical skill source imported from another project
├── AGENTS.md                 # Development instructions for this project
├── PLANS.md                  # ExecPlans methodology
├── docs/                     # Project documentation and the ExecPlans index
├── opencode.json             # Project-local OpenCode configuration
├── .agents/skills/           # Development helper skills, not release contents
├── .opencode/                # Local agent/plugin definitions and initialization kit
├── .ignore                   # Agent context exclusions
└── skills-lock.json          # Sources recorded by the skills installer
```

## Key components

| Component | Path | Purpose |
| --- | --- | --- |
| Tilda skill | [`tilda/SKILL.md`](../tilda/SKILL.md) | Defines tool discovery, safe site work, and when to check live official documentation. |
| Local development tools | [`.agents/skills/`](../.agents/skills/) and [`.opencode/`](../.opencode/) | Support authoring and review; do not ship them as part of the Tilda skill. |
| Planning | [ExecPlans index](exec-plans/README.md) | Tracks active, completed, and deferred work. |

## Maintenance rule

Update routes, tree and descriptions when structure, responsibilities,
entrypoints or test locations change. Keep this a navigator, not a full inventory.
