---
id: project-abt
kind: project
topics: [tools, automation, self-hosting, saas]
visibility: agents-only
last-verified: 2026-09-06
links:
  site:
  backend: https://tools.ceeveeglobal.com
  repo: https://github.com/dimu2000/Python_fast_api
---
# ABT — AIBuiltTools

## What it is (elevator pitch)
A WordPress site that runs a catalogue of AI-backed utilities for WordPress owners —
plugin and performance analysis, security and config auditing, file utilities, SEO and
content tools — paid for out of a points balance rather than per-call cash. Points are
earned (welcome bonus, daily login, referrals) and spent on tool runs; as of 2026-07 they
can also be spent to *own* things, buying EDD digital products outright.

## Current status
Internally complete, not publicly launched. As of 2026-07-15 all 17 tools were live
end-to-end — FastAPI backend deployed, WordPress dispatcher configured, pages published,
a `/tools/` hub page built and one "Tools" menu item added. The whole site was still
gated behind `site_maintenance_mode`, so none of it was publicly visible yet.

## Key numbers
- 17 tools built and verified end-to-end through the live dispatcher (2026-07)
- 15 exposed on the `/tools/` hub across 4 categories; 16 pages published, 15 tools plus
  Account (2026-07)
- 2 tools (WP Error Expert, WP Error Expert Enhanced) deliberately left in draft — live in
  the API, never exposed or linked (2026-07)

Every number carries the month it was true.

## Architecture / how it works
Two halves. WordPress side: mu-plugins — `abt-tool-definitions.php` (per-tool dispatcher
config), `abt-tool-runner.php` (shared front-end runner and field types),
`abt-points-purchase.php` (points checkout for EDD downloads, opt-in per product via a
`_abt_points_price` postmeta). All tool calls go through `admin-ajax.php`, with nonce →
rate limit → lock → balance check → deduct-before-grant with refund-on-failure. Points
ledger is MyCred; products are EDD.

Backend: a shared FastAPI service at `tools.ceeveeglobal.com`, one router per tool.
**It does not deploy from git.** Coolify is configured `build_pack: dockerimage` and only
pulls a prebuilt image from the self-hosted registry `docker.aibuilttools.com`; the GitHub
repo above is source-of-record only. See
[[lesson-2026-07-deployed-not-deployed-coolify]].

## Decisions & constraints
- Points buttons render *alongside* the real-money EDD buy button, never replacing it.
- Tool discovery is a hub page, not a 15-item nav dropdown.
- Known, accepted trade-off (2026-07): a $0-cash points purchase still counts as a
  qualifying first purchase for the referral reward. Flagged, not solved — consistent with
  "a purchase is a purchase," but it would need a real-money floor to change.
- `app/config.json` holds real secrets baked into the image and read from disk with no
  env-var alternative — a pre-existing pattern, never staged into git.

## Pointers
- Build log archive: `raw/doc-2026-09-abt-engineering-journal.md`
- Related notes: [[lesson-2026-07-abt-real-path-testing]],
  [[lesson-2026-07-secret-redaction-magic-quotes]],
  [[lesson-2026-07-deployed-not-deployed-coolify]],
  [[lesson-2026-07-read-the-installed-source]]
