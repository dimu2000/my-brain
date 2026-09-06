---
id: take-2026-09-modular-scripts-no-monoliths
type: take
topics: [engineering-thinking, automation, python]
projects: [cvg]
source: repo-2026-09-cvg-project-contract
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---

# One job per script, one thing per plugin — no monoliths

Modularity is a standing rule in the CVG contract, not a preference applied when convenient. It
applies to Python scripts and to WordPress mu-plugins alike, and it is stated as a constraint on
how work gets split before it gets written.

> VERBATIM: "**Always create modular, reusable code.** No monolithic scripts. Each script does one job. Each mu-plugin does one thing. Split big tasks into small, testable pieces."

The content machine shows the shape this produces: draft generation, publishing, pin-brief
generation, image pickup and webp conversion each live in their own script, chained by files on
disk rather than folded into one runner. That is also what makes a failure survivable — when pin
generation fails after a publish, it is logged as a warning with the exact rerun command and never
blocks the WordPress publish.

Source doc: `raw/repo-2026-09-cvg-project-contract.md`. Related: [[take-2026-09-no-image-generation-api-in-scripts]]
