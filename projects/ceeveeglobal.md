---
id: project-ceeveeglobal
kind: project
topics: [content-creation, seo, automation, entrepreneurship]
visibility: agents-only
last-verified: 2026-09-06
links:
  site: https://ceeveeglobal.com
  repo:
  docs:
---
# CeeVeeGlobal.com

## What it is (elevator pitch)
The main CVG property: a WordPress tutorial blog covering WordPress fixes,
self-hosting and AI tooling, monetised through Google AdSense and affiliate
links, with email capture feeding a self-hosted Mautic list. It is also the
front door for two newer things — free browser tools that pull SEO traffic
(a domain name generator), and CVG Web Factory, a productised web-design
service selling AI-built sites on a fixed build fee plus a recurring Care
Plan.

## Current status
Active and the only property earning today. Affiliate layer is built: Auto
Affiliate Links configured, a "Tools I Use" page and a footer disclosure
mu-plugin shipped 2026-07-01. The free domain name generator is live at
`/free-domain-name-generator/` with a secure REST proxy, rate limiting,
Namecheap affiliate links and FAQPage + HowTo + WebApplication schema. CVG
Web Factory launched 2026-08-10 — the `/web-design` sales page, instant-quote
estimator, lead-capture backend and a sitewide promo banner are all live,
with **no leads yet**. The companion YouTube channel is still short of the
1,000-subscriber YouTube Partner Program threshold; Medium is not activated.

Open blockers, all on CVG: Zoho SMTP is blocking quote-request notification
emails ("Unusual sending activity detected") — leads are safe in the
`wp_cvg_quote_requests` table regardless, only the notification is affected;
the `primary` nav location has no menu assigned (pre-existing), so
`/web-design` is reachable only from the banner, not the header; hero and
portfolio images are waiting on Dimu to generate and drop in.

## Key numbers
- 670 YouTube subscribers — 330 short of the 1,000 needed for YPP (2026-06)
- 21 affiliate links configured via Auto Affiliate Links (2026-06)
- 3 programs actually in PrettyLinks: Elementor Pro, Contabo, Hostinger (2026-06)
- 13 tools across 4 categories on the "Tools I Use" page (2026-07)
- 11,432 bot accounts deleted; user registration now disabled (2026-06)
- Brevo free tier: 300 emails/day, 9K/month (2026-06)
- (agents-only: revenue targets are unmet planning figures, not earnings —
  affiliates $100–500/mo, YouTube ads $50–200/mo, Medium Partner $20–100/mo,
  and CVG Web Factory $2,100+/mo recurring at 30 Care Plan clients per
  `CVG-Web-Factory-Manual.pdf`.)

## Architecture / how it works
WordPress hosted in Docker on a Coolify VPS. Content is produced by a Flask
dashboard pipeline (Keyword Pool / Personal Journey → Daily Plan → Review
Queue → Publish + Schedule) which is the canonical content engine, driven
alongside Claude Code skills (`wp-seo`, `video`, `wp-site`, `social`,
`prog-seo`, `wp-backend`, `wp-frontend`, `wp-mobile`) and slash commands.

(agents-only: since 2026-08-06 the whole project runs on an Optiplex home
server — dashboard at `http://192.168.1.207:5050`, LAN-only, always on via
`cvg-dashboard.service`, with the Error Library pipeline back on a daily
04:00 cadence via `cvg-error-pipeline.timer` after the old Windows Task
Scheduler trigger stopped working post-migration. Deliberately not wired
into Coolify beyond the two live WordPress sites.)

Email runs on Brevo SMTP (`smtp-relay.brevo.com:587`, SPF + DKIM + DMARC
authenticated on the domain) into self-hosted Mautic at
`mautic.aibuilttools.com`, with the opt-in form injected after paragraph 3
on every single post. Automation is self-hosted n8n: the Gumroad-sale →
Mautic workflow is active; WordPress RSS → Facebook is imported but inactive
(needs a Facebook Page token) and YouTube RSS → Mautic broadcast needs manual
activation. Web Factory runs on its own mu-plugins (`cvg-web-design-page.php`,
`cvg-estimator.php`, `cvg-quote-handler.php`, `cvg-web-design-banner.php`)
with leads landing in a custom `wp_cvg_quote_requests` table.

## Decisions & constraints
- Staging was decommissioned 2026-06-13; `push-staging-to-live.sh` is dead
  and must not be run.
- The dashboard stack stays Optiplex-only, per Dimu's instruction — not
  migrated into Coolify.
- Mail reputation rule: Postal never runs on the same VPS as WordPress or
  Mautic (shared IP = reputation risk). Scaling path is Brevo free → Brevo
  Starter at 2K contacts → Postal on a separate $5/mo VPS at 10K+.
- Medium repurposing must use a different angle and a canonical URL back to
  CVG, to protect CVG's own SEO rather than compete with it.
- The estimator was recoloured to the site's real brand orange (`#ff6302`)
  rather than shipping the standalone draft's palette.
- (agents-only: high-commission affiliate programs — Cloudways, WPEngine,
  Kinsta, Astra Pro, WP Rocket, Semrush, GeneratePress — are identified but
  NOT signed up. Treat as an unexecuted plan, never as a current partnership.)

## Pointers
- Planning docs (live outside this repo): `MASTER-PLAN.md`, `JOURNEY.md`,
  `CVG-Web-Factory-Manual.pdf`
- Source archive pointer: `raw/repo-2026-09-cvg-master-plan.md`
- Sister project: `projects/aibuilttools.md`
- Related notes: none yet
