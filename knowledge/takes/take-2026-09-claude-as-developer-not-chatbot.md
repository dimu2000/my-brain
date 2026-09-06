---
id: take-2026-09-claude-as-developer-not-chatbot
type: take
topics: [ai-agents, llm-engineering, wordpress, automation]
projects: [ceeveeglobal]
source: doc-2026-09-cvg-origin-story
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---
# Use Claude as an actual developer, not a chatbot

The whole CeeVeeGlobal rebuild rests on one position: an AI you can only
paste code into is not a developer. To do the job it needs the access a
developer has — the files, the database, and the error log.

> VERBATIM: "Decided to use AI (Claude) not just as a chatbot but as an actual developer"

What that meant in practice: VS Code connected to the Coolify VPS over SSH,
a `CLAUDE.md` briefing file read every session, and a small Node.js MCP
server on the VPS exposing `read_debug_log`, `query_database`,
`list_plugins` and `get_snippets`. With those wired in, Claude reads the
plugins, snippets and theme files, queries the database directly and reads
`debug.log` — then writes a full site audit before touching anything.

Setup detail: `fact-2026-09-cvg-claude-code-setup`. Full source:
`raw/doc-2026-09-cvg-origin-story.md`.
