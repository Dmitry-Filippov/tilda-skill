# Tilda Skill

An [Agent Skill](https://agentskills.io/) for working with Tilda Publishing. Get guidance from official Tilda documentation, or use Tilda MCP to make authorized changes to a site.

## Install

```sh
npx skills add Dmitry-Filippov/tilda-skill --skill tilda
```

To install manually, copy the [`tilda/`](tilda/) directory into your client's skills folder. In an OpenCode project, the resulting path is `.agents/skills/tilda/SKILL.md`; other clients may use a different folder.

## Try it

- “How do I set up redirects in Tilda? Link me to the official docs.”
- “Update the heading on this page and save it as a draft. Don't publish.”
- “What do I need to do before moving my domain to Tilda?”

## When you need MCP

You don't need a Tilda account or MCP to ask for guidance. To inspect or edit a site, connect the [official Tilda MCP](https://help.tilda.cc/mcp) and grant access to the relevant project. If an operation isn't available through MCP, the skill explains what's blocked and suggests documented manual steps when available.

Any change that goes live requires explicit authorization. For a draft-only request, the skill stops before saving if the edit could go live immediately. It reports separately what was saved and what it could verify on the published page.
