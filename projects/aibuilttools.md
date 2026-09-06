---
id: project-aibuilttools
kind: project
topics: [saas, tools, automation, python]
visibility: agents-only
last-verified: 2026-09-06
links:
  site: https://aibuilttools.com
  repo:
  docs:
---
# AIBuiltTools.com

## What it is (elevator pitch)
Two businesses on one WordPress site: an Easy Digital Downloads store selling
scripts, kits and source-code bundles, and a metered AI-tool platform where
logged-in users spend points to run browser-based tools (WordPress
diagnostics, file/document utilities, SEO analysis) backed by a FastAPI
service. Points are bought as packs or granted by subscription, and — since
Phase 9 — can also be spent to *own* a digital product, not just to run a
tool.

## Current status
Built but not launched. The site is still in coming-soon / maintenance mode
(`site_maintenance_mode`), so the published tool pages are not publicly
visible. EDD has recorded no sales, and Gumroad has 0 sales.

Points-system phase tracker: Phases 0–4 (foundation hardening, one-wallet
point economy, PayPal payments, subscription allowance engine, shared
dispatcher v2), Phase 7 (growth loops) and Phase 9 (buy-with-points for EDD
products) are done and verified. Phase 5 (user-facing layer) and Phase 6
(tool rollout) are in progress; Phase 8 (monitoring/admin) is not started.

What that means concretely: 15 tools are live end-to-end with real point
deduction, a `/tools/` app-shell hub groups them into 4 categories, and
`/shop/` plus `/point-packages/` are live with working EDD checkout. Still
outstanding before a real launch — PayPal is in sandbox and needs a live app;
the `/account/` page is still draft; `wp_mail()` has no working transport at
all, so account-creation emails are silently failing today; and Phase 5.3
email automations are deferred pending either an authenticated Mautic API
user or a direct Brevo transactional integration.

## Key numbers
- 15 tools live on the `/tools/` hub across 4 categories (2026-07)
- 17 dispatcher tool IDs allocated (`CUSTOM_TOOL_ID` 1–17) (2026-07)
- Point packs priced $3 / $7 / $15, matching `ABT-POINTS-PRICING.md` (2026-07)
- 4 PayPal billing plans created and active — sandbox only (2026-07)
- Allowance engine: idempotent per calendar month, 1,500-point rollover cap (2026-07)
- Growth loops: 25-point welcome bonus, 5-point daily login bonus capped at
  50 lifetime, plus a custom referral reward (2026-07)
- Stack versions: EDD v3.6.8, MyCred v3.2.1 with exactly one `abt_points`
  wallet and all 17 default add-ons force-disabled (2026-07)
- 0 sales to date (2026-07)
- (agents-only: target is $200+/month from the points and subscription
  system — a goal funding tool development, not current revenue.)

## Architecture / how it works
WordPress in the Coolify container `wordpress-mos8wwkckwg4wo0cg8gc4w08`, Neve
child theme, EDD + PayPal gateway. The points system is a stack of mu-plugins:
`abt-points-guard.php` (config guard, concurrency lock, rate limit, generic
errors), `abt-points-common.php` + `abt-tool-dispatcher.php` +
`abt-tool-definitions.php` + `abt-tool-runner.php` (the shared handler),
`abt-subscription-allowance.php`, `abt-points-growth-hooks.php` and
`abt-points-purchase.php`. The dispatcher deducts before the API call and
refunds on failure, always releases its lock, and supports file-in (multipart
upload, base64 to the API) and file-out (a `file_base64` + `filename` result
convention that auto-renders as a download link).

Tool logic lives in a separate FastAPI service (working repo `D:\fastapi-live`,
served at `tools.ceeveeglobal.com`). (agents-only: deploys are NOT git-triggered
— Coolify pulls a pre-built image from `docker.aibuilttools.com`, so a plain
`git push` does nothing; the image is built and tested on the VPS, then cut
over with a rollback container preserved.) Buy-with-points creates a genuine
EDD order via `edd_insert_payment()` + `edd_update_payment_status()` at $0 on
an `abt_points` gateway, so purchases appear normally in Purchase History and
satisfy `edd_has_user_purchased()`.

## Decisions & constraints
- Payments went to PayPal rather than a paid subscriptions plugin: it fits
  Sri Lanka payment access, adds no paid plugin, and stays lightweight —
  point packs via EDD+PayPal checkout, subscriptions via PayPal's own
  Subscriptions REST API.
- Exactly one point type. The referral system was built custom instead of
  using MyCred's Referral add-on specifically to preserve that discipline.
- WP Error Expert and WP Error Expert Enhanced are paused per Dimu — kept in
  the API, left in draft, not exposed or linked.
- Wave 3 tools are skipped for now: they need a LibreOffice/Docker image
  change, deferred as a separate decision.
- GSC Analyzer is deliberately scoped as a CSV-export analyzer, not a live
  OAuth integration.
- Phase 6's FastAPI-side `X-ABT-User-ID` usage-logging middleware is deferred
  to Phase 8 — it is optional analytics, not required for points to work.
- Accepted trade-off, flagged not gated: a $0-cash points purchase still
  counts as a qualifying "first purchase" for the Phase 7 referral reward.

## Known open issues
- Certificate Bulk Generator's font availability on the real Linux production
  container is unverified.
- CV AI Transcriber Pro's external stylesheet (jsDelivr/GitHub) is 404'ing —
  flagged, not fixed.
- The contact page's hardcoded personal email was removed, pending Dimu's
  replacement address.
- Office Docs Toolkit not started.

## Pointers
- Planning docs (live outside this repo): `CVG-POINTS-MASTER-PLAN.md`,
  `ABT-POINTS-PRICING.md`, `JOURNEY.md`
- Source archive pointer: `raw/repo-2026-09-cvg-master-plan.md`
- Sister project: `projects/ceeveeglobal.md`
- Related notes: none yet
