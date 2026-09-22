---
id: project-cvg
kind: project
topics: [content-creation, automation, self-hosting, seo]
visibility: agents-only
last-verified: 2026-09-22
links:
  site: https://ceeveeglobal.com
  store: https://aibuilttools.com
  docs:
---
# CVG Project — two WordPress sites and one content machine

## What it is (elevator pitch)
Two WordPress sites built and run by Dimuthu (Dimu) in Sri Lanka, fed by a single automated
content machine. **ceeveeglobal.com (CVG)** is the tutorial blog — AdSense, affiliates, a course.
**aibuilttools.com (ABT)** is the digital product store — WordPress plugins, Python scripts, AI
prompt packs, HTML/JS apps — selling through Easy Digital Downloads and PayPal, with CVG posts
cross-linking into it.

## Current status
Both sites live on a Coolify VPS (Docker, Neve child theme on each). ABT has no separate staging
and is worked on directly under coming-soon mode. CVG's staging was decommissioned 2026-06-13; its
review gate now lives in the dashboard. The project itself has run from Dimu's Optiplex home
server since 2026-08-06.

## Key numbers
None in this source — the contract doc carries no traffic, revenue or sales figures.

## Architecture / how it works
The content engine is a Flask dashboard, running persistently on the home server:

```
Idea Pool (keywords + personal-journey work)
  → Daily Plan → generated draft (post.md + image-prompts.md)
  → Review Queue (rendered preview, chat edits, image upload)
  → Publish/Schedule direct to ceeveeglobal.com
  → Repurpose → video / shorts / social
```

A separate Error Library pipeline runs daily on a timer and creates WordPress drafts; publishing is manual in wp-admin (auto-publish retired 2026-09-20).
A Pinterest pin pipeline generates 3–4 pin briefs per published post, writes them to a shared
`image-gallery/<batch>/` convention, and a watcher uploads each hand-made image to object storage
and queues a `pins_queue` document for a not-yet-built n8n publishing workflow.
(agents-only: the Pinterest app is still pending Trial access approval, so the n8n side is
deliberately deferred.)

## Decisions & constraints
Ten Golden Rules govern the build. The load-bearing ones:
- Scripts don't call an image-generation API, for now — see [[take-2026-09-image-apis-not-for-now]].
- Modular, reusable code only; no monolithic scripts — see [[take-2026-09-modular-scripts-no-monoliths]].
- Content reaches live only through the Review Queue; site/code changes on live need an explicit
  go-ahead — see [[lesson-2026-09-review-queue-replaced-staging]].
- No hardcoded credentials, and no secrets in any tracked file — real values live only in `.env`,
  docs use placeholders.
- Every completed task is logged to JOURNEY.md in enough detail to become a post or video, and
  MASTER-PLAN.md checkboxes are updated as work lands.

## Pointers
- Contract, plan and build log (MASTER-PLAN.md, JOURNEY.md, skills, scripts): in the project repo
  on the home server — not mirrored into this mind.
- Source pointer: `raw/repo-2026-09-cvg-project-contract.md`
- Where it runs: [[fact-2026-09-project-runs-on-optiplex-home-server]]
