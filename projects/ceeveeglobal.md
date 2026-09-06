---
id: project-ceeveeglobal
kind: project
topics: [wordpress, automation, self-hosting, content-creation]
visibility: agents-only
last-verified: 2026-09-03
links:
  site: https://ceeveeglobal.com
---
# CeeVeeGlobal

## What it is (elevator pitch)
CeeVeeGlobal.com is my WordPress site — solutions, tutorials and AI-powered
tools. It is being rebuilt from a slow, snippet-heavy site into a structured
one: a digital shop for PDFs, eBooks and Python apps, blog posts created
automatically, and a Claude assistant on the site for visitors.

## Current status
Mid-rebuild. Setup steps 1–4 (SSH, `CLAUDE.md`, MCP server, MCP connected to
Claude Code) completed 2026-05-31. The full site audit (step 5) is done and
changes on staging (step 6) have begun — the build log records no dates for
either. The shop, auto-posting and on-site assistant are goals, not shipped
features.

## Key numbers
None recorded in the source.

## Architecture / how it works
WordPress on a Coolify VPS (Docker), Claude API for AI features, n8n for
automation workflows, Easy Digital Downloads for the digital shop, WP REST
API for blog publishing, Redis object cache for speed. Development runs in
Claude Code + VS Code over Remote SSH.
(agents-only: a Node.js MCP server on the VPS at
`/home/coolify-service/wp-mcp/` exposes `read_debug_log`, `query_database`,
`list_plugins` and `get_snippets`; Claude Code reaches it via
`.claude/mcp.json`.)

## Decisions & constraints
- Site changes: staging first → auto-test → auto-push to live. Blog posts go
  straight to live via the WP REST API.
- The point system is removed entirely — code and database.
- Proper plugin structure over PHP snippets; snippets running on every page
  were the main speed problem.
- Day-to-day task automation (blog posting etc.) planned but deliberately
  not built yet: main path first.

## Pointers
- Build log: `JOURNEY.md` in the WordPress project root on the VPS
- Source archive: `raw/doc-2026-09-cvg-origin-story.md`
- Related notes: `take-2026-09-claude-as-developer-not-chatbot`,
  `story-2026-09-ceeveeglobal-rebuild-origin`,
  `fact-2026-09-cvg-claude-code-setup`,
  `take-2026-09-code-staging-content-live`
