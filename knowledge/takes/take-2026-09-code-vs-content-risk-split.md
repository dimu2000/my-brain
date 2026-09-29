---
id: take-2026-09-code-vs-content-risk-split
type: take
topics: [wordpress, automation, engineering-thinking]
projects: [ceeveeglobal, cvg]
source: repo-2026-09-cvg-project-contract
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---

# Code and content still split by risk — the mechanism changed, not the split

The principle holds: code changes and content changes don't get the same path to live. What
changed is *how* each is gated, because staging is gone (decommissioned 2026-06-13).

**Content** publishes straight to live, gated by human review in the Dashboard Review Queue —
nothing reaches the site until Dimu has reviewed the rendered preview and clicked Publish.

> VERBATIM: "Drafts get reviewed before they go live... nothing reaches the live site until Dimu has reviewed the rendered preview there and clicked Publish."

**Code/site changes** (theme, plugins, snippets, DB) also go directly on live now — there's no
staging to test them on first — but they're gated by a different mechanism: Dimu's explicit
go-ahead per change, not an automated staging test.

> VERBATIM: "Never make site/code changes (theme, plugins, snippets, DB) directly on live without Dimu's go-ahead"

Both paths now skip staging entirely; the risk split survives as two different human-gate
mechanisms instead of one automated one plus one direct one.

Supersedes [[take-2026-09-code-staging-content-live]]. Content-side detail (the Review Queue,
the Error Library's move to drafts-only):
[[lesson-2026-09-no-ungated-publish-path]]. Source: `raw/repo-2026-09-cvg-project-contract.md`
(Golden Rules 1 and 7, ceeveeglobal-site-creation/CLAUDE.md).
