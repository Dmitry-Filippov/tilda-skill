# Product

## What the product is about

`tilda` is an agent skill for answering Tilda questions and carrying out authorized work on Tilda sites. This project owns the source and is the place to refine and validate it before release. The skill is not a website or a replacement for access to the Tilda editor.

## Target audience

Site owners, designers, and developers working with an AI agent on Tilda, including teams migrating a site to Tilda.

## Main features

- Guidance for pages, blocks and Zero Block, forms, catalogs, SEO, domains, and publishing.
- Relevant official documentation for uncertain, unfamiliar, or potentially outdated guidance.
- Authorized actions through available Tilda MCP tools, with documented manual alternatives when an operation is unsupported.

## Key user scenarios

1. Get current instructions for a Tilda feature and a link to the applicable official article.
2. Ask the agent to inspect and update an authorized site feature, then report separately what was saved, published, and verified.
3. Identify which migration or setup steps need the user's participation without sharing secrets in chat.

## Technical facts

The release source is [`tilda/SKILL.md`](../tilda/SKILL.md). Live actions require a compatible Tilda MCP connection, the necessary site permissions, and explicit task scope. The distribution channel and live test environment are not established yet.
