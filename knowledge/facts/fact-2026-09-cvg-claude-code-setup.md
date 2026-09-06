---
id: fact-2026-09-cvg-claude-code-setup
type: fact
topics: [wordpress, llm-engineering, self-hosting, automation]
projects: [ceeveeglobal]
source: doc-2026-09-cvg-origin-story
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---
# The 6-step setup that gives Claude Code live access to CeeVeeGlobal

1. Connect VS Code to the Coolify VPS with the Remote SSH extension and open
   the WordPress project folder directly on the server.
2. Write `CLAUDE.md` in the project root — site structure, DB layout, key
   files, goals, coding style — read by Claude Code every session.
3. Build a small Node.js MCP server on the VPS exposing `read_debug_log`,
   `query_database`, `list_plugins` and `get_snippets`.
4. Add `mcp.json` to the VS Code project settings so Claude Code can call
   those tools mid-conversation: files plus live DB data.
5. Claude audits the staging site deeply — plugins, snippets, theme files,
   point system tables, `debug.log` — and produces a full audit report.
6. Claude makes changes per instruction on staging, auto-tests them (errors,
   page loads), and pushes to live once staging is clean.

Surrounding stack: WordPress on Coolify VPS (Docker), Claude API, n8n, Easy
Digital Downloads, WP REST API, Redis object cache.
