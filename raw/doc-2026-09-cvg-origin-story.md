---
id: raw-doc-2026-09-cvg-origin-story
kind: raw
title: Why the CeeVeeGlobal rebuild started (origin section of JOURNEY.md)
source: doc-2026-09-cvg-origin-story
source_url: null
date: 2026-09
visibility: agents-only
---
# Archive — origin section of JOURNEY.md (CeeVeeGlobal build log)

Unpublished internal build log, fed directly on 2026-09-03. No URL exists;
this archive is the provenance for the notes citing
`doc-2026-09-cvg-origin-story`.

---

## Who is Dimuthu (Dimu Harshana)?
- Digital entrepreneur, 15+ years in IT
- 8+ years specializing in WordPress, automation, and AI tools
- Runs CeeVeeGlobal.com (WordPress solutions, tutorials, AI-powered tools)
- Also runs AIBuiltTools.com
- YouTube channels: "CeeVee Global" (WordPress/AI tutorials), "Nature Lapse" (nature docs)
- Based in Sri Lanka
- Teaching style: start simple, build fast, learn by doing

---

## The Problem (Why We Started This Rebuild)

Dimu's WordPress site (CeeVeeGlobal.com) had grown messy over time:
- Point system that was no longer needed
- Too many PHP code snippets running on every page — site was very slow
- No digital shop yet (wanted to sell PDFs, eBooks, Python apps)
- Blog posts still created manually
- No AI integration on the site itself
- No proper structure or standards

---

## The Vision (What We Decided to Build)

After a deep planning conversation, we decided on a complete rebuild with:

1. Remove the point system cleanly (code + database)
2. Optimize all PHP snippets for speed
3. Add a digital shop (Easy Digital Downloads) for PDFs and Python apps
4. Auto blog post creation — give Claude a topic, full post published automatically
5. Claude AI assistant integrated into the site for visitors
6. Proper WordPress plugin structure (no hacks, no messy snippets)

---

## The Tech Stack Chosen

- WordPress on Coolify VPS (Docker)
- Claude API (Anthropic) for AI features
- n8n for automation workflows
- Easy Digital Downloads for digital shop
- Claude Code + VS Code for development
- MCP server (Node.js) connecting Claude to WordPress DB
- WP REST API for blog publishing
- Redis object cache for speed

---

## The Setup Plan (6 Steps)

### Step 1 — Connect VS Code to VPS via SSH
- Install Remote SSH extension in VS Code
- Connect to Coolify VPS using IP + SSH key
- Open WordPress project folder directly on server

### Step 2 — Create CLAUDE.md
- A briefing file Claude Code reads every session
- Contains: site structure, DB layout, key files, goals, coding style
- Saved in WordPress project root on VPS

### Step 3 — Build WordPress MCP Server
- Small Node.js server running on VPS
- Exposes tools: read_debug_log, query_database, list_plugins, get_snippets
- Gives Claude live access to site data

### Step 4 — Connect MCP to VS Code
- Add mcp.json to VS Code project settings
- Claude Code can now call MCP tools mid-conversation
- Full context: files + live DB data

### Step 5 — Claude Analyzes Staging Site Deeply
- Claude reads all plugins, snippets, theme files
- Queries database for point system tables
- Reads debug.log for existing errors
- Creates a full site audit report

### Step 6 — Claude Makes Changes Per Instructions
- All changes go to staging site first
- Claude auto-tests (checks errors, page loads)
- Once staging is clean → auto-push to live
- Blog posts go directly to live via WP REST API

---

## Workflow Rules We Set

- **Site changes**: staging first → auto-test → auto-push to live
- **Blog posts**: direct to live site via WP REST API (content only, no code risk)
- **Day to day tasks** (blog posting etc): planned but not built yet — focus on main path first

---

## Journey Timeline

| Date | Milestone |
|------|-----------|
| 2026-05-29 | Full rebuild plan created in Claude.ai |
| 2026-05-31 | Step 1: SSH connected (coolify-service@194.163.177.244) |
| 2026-05-31 | Step 2: CLAUDE.md + JOURNEY.md placed on staging VPS root |
| 2026-05-31 | Step 3: MCP server built at /home/coolify-service/wp-mcp/ |
| 2026-05-31 | Step 4: MCP connected to Claude Code via .claude/mcp.json |
| - | Step 5: Full site audit done |
| - | Step 6: Changes begin on staging |

---

## Notes for Future Blog Post

This journey is a real story of a solo digital entrepreneur who:
- Had a slow, messy WordPress site built up over years
- Decided to use AI (Claude) not just as a chatbot but as an actual developer
- Set up a proper AI-powered development workflow from scratch
- Rebuilt the entire site structure with Claude Code doing the heavy lifting
- Created a system where Claude can write and publish blog posts automatically

**Potential blog post titles:**
- "How I Used Claude AI to Completely Rebuild My WordPress Site"
- "From Messy WordPress to AI-Powered Website in 6 Steps"
- "I Let Claude Code Manage My WordPress Site — Here's What Happened"
- "The Real MCP WordPress Setup That Actually Works"
