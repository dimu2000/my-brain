---
id: take-2026-09-code-staging-content-live
type: take
topics: [wordpress, automation, engineering-thinking]
projects: [ceeveeglobal]
source: doc-2026-09-cvg-origin-story
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---
# Code goes through staging; content goes straight to live

The rule that lets an AI push to a production site is that the two kinds of
change are not treated the same. Anything touching code lands on staging
first, gets auto-tested for errors and page loads, and is pushed to live
only once staging is clean.

Blog posts skip all of it and publish directly to the live site through the
WP REST API.

> VERBATIM: "direct to live site via WP REST API (content only, no code risk)"

The split is by risk, not by convenience — which is also why the day-to-day
task automation (blog posting and the like) was planned but left unbuilt:
main path first.
