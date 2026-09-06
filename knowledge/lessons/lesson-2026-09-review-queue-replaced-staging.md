---
id: lesson-2026-09-review-queue-replaced-staging
type: lesson
topics: [automation, content-creation, engineering-thinking]
projects: [cvg]
source: repo-2026-09-cvg-project-contract
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---

# Killing staging didn't remove the gate — it moved it into the dashboard

CVG's staging environment was decommissioned on 2026-06-13 (container and volume removed, not to
be restarted). Content now publishes straight to the live site, but the human checkpoint survived
the teardown by moving into the dashboard.

> VERBATIM: "**Drafts get reviewed before they go live.** CVG staging was decommissioned 2026-06-13 — content publishes directly to ceeveeglobal.com. The review gate is now the **Dashboard Review Queue** (`dashboard.py` → `http://localhost:5050` → Review Queue page): nothing reaches the live site until Dimu has reviewed the rendered preview there and clicked Publish."

Two boundaries make the design work. Content and code are separate tracks: content passes through
the Review Queue, while theme/plugin/snippet/DB changes on live still require an explicit
go-ahead per Golden Rule 7. And the gate is scoped, not universal — the Error Library pipeline
deliberately auto-publishes live with no review step.

The cost of the migration is documentation drift: the older slash commands still exist as a manual
fallback but their docs predate the shutdown, one is marked stale and one dead because it targets
the removed staging container. The dashboard pipeline is the source of truth.

Source doc: `raw/repo-2026-09-cvg-project-contract.md`
