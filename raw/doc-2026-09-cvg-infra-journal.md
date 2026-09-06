---
id: doc-2026-09-cvg-infra-journal
kind: raw
source: doc-2026-09-cvg-infra-journal
source_url: null
date: 2026-09
visibility: agents-only
---
<!--
  Archive of the fed source: five dated entries from the CVG build log,
  newest first. Fed 2026-09-06 with no URL — this file is the provenance
  anchor for the notes citing source: doc-2026-09-cvg-infra-journal.
  The 2026-07-20 Pinterest entry was archived but not extracted into notes.
-->

# CeeVeeGlobal.com — Rebuild Journey Log

---
## VPS Disk Rescue — 82 GB of Runaway UpdraftPlus Backups Cleared, Pruning Actually Fixed — 2026-09-02
**Site:** CVG (ceeveeglobal.com) — VPS-wide
**Phase:** Infrastructure / server health

### What was done
A routine "check my server resources" turned up a VPS disk at 76% and climbing ~1 GB/day. Root cause: UpdraftPlus had been keeping **every single daily backup since 28 May 2026** — 71 backup sets, 1,514 files, **93 GB** — despite retention being set to 1. Fixed the setting that was silently disabling pruning, deleted the 64 oldest sets, and resynced the plugin's history. Disk went from **76% → 33%**, reclaiming ~82 GB.

### Why
The VPS had ~48 GB free and was losing ~1 GB/day. At that rate the disk would have filled in roughly 7 weeks, which would have taken down **all 35 containers** — CVG, ABT, Coolify itself, n8n, Mautic, MinIO — not just the blog. The backups were 130× the size of the site they were backing up (uploads is only 717 MB).

### The actual root cause
`updraft_retain` was already `1`, so this looked configured correctly at first glance. The UpdraftPlus log gave it away — this line appeared on every run:

> `Prune old backups from local store: nothing to do, since the user disabled local deletion and we are using local backups`

The `updraft_delete_local` option was `0`. With no remote storage configured (`updraft_service` was empty — backups are local-only), that setting makes UpdraftPlus **skip retention pruning entirely**. The retention number was never being consulted. Setting `updraft_retain` alone would have fixed nothing.

### Steps taken
1. Diagnosed from the top down: `df -h` → `docker system df` (volumes = 116 GB) → `du` per volume → `n0g4skoo8sgwgc848koog0ww_wordpress-files` = 94 GB → `wp-content/updraft` = 93 GB.
2. Read the newest UpdraftPlus log and found the "user disabled local deletion" line, which explained why `updraft_retain=1` had been ignored for 96 days.
3. Snapshotted the current option values to `/root/updraft-options-before-20260902.txt` on the VPS as a rollback record.
4. Applied: `updraft_delete_local` `0`→`1` (the real fix — re-enables pruning), `updraft_retain` `1`→`7`, `updraft_retain_db` `1`→`7`. Safe with no remote service configured: the "delete after upload" path only fires after a successful *remote* upload, so the fresh local backup is never removed.
5. Dry-ran the deletion first (1,321 files / 81.8 GB to delete; 147 files / 9.1 GB across 7 complete daily sets to keep), confirmed the newest set (2026-09-01) had a complete file list including `-db.gz`, then deleted.
6. Pruned `updraft_backup_history` — `UpdraftPlus_Backup_History::rebuild()` only *adds* sets found on disk, it does not drop entries whose files are gone, so a small `wp eval-file` script removed the 63 stale entries. History went 70 → 7, matching disk. Temp script deleted afterward.
7. Removed four stale `wp-content` leftovers untouched since May/June: `Reset images_2_25.zip-old`, `wpvividbackups-old`, `wflogs-old`, `uploads-webpc-old`.
8. Verified: disk 33%, CVG returns HTTP 200, container healthy, all 98 published posts intact, no new entries in debug.log.

### Result
Disk `/` at **63 GB of 193 GB (33%)**, 130 GB free — up from 48 GB. Docker volumes 116 GB → 28 GB. Backups now self-prune to 7 daily sets (~9 GB steady state) instead of growing forever. CVG and ABT both unaffected throughout.

### Known follow-ups (not blocking, flagged for Dimu)
- **Swap is still 3.8 GB of 4.0 GB used** — untouched by this cleanup, it's a memory issue not a disk one. Six separate `mysqld` instances on an 11 GB box; the CVG one alone is swapping ~960 MB. Load sits at 4–7 on 6 cores. Worth a separate tuning pass (per-instance `innodb_buffer_pool_size`).
- **Backups are local-only** — `updraft_service` is empty, so all backups live on the same disk as the site. A disk failure loses both. Worth pointing UpdraftPlus at the existing MinIO/S3 bucket.
- One live `Reset images_2_25.zip` (128 MB) left in `wp-content` — deliberately kept the single remaining copy rather than deleting the file outright; unclear what it's for.
- `wpvividbackups/` (120 MB) still present — a second backup plugin's output alongside UpdraftPlus. Possibly redundant.

### Blog/video angle
"Why your WordPress backup plugin is quietly eating your server — the UpdraftPlus setting that silently disables retention"

### Screenshots to take
- [ ] `df -h` before (76%) and after (33%)
- [ ] The UpdraftPlus log line: "nothing to do, since the user disabled local deletion"
- [ ] UpdraftPlus "Existing backups" tab showing 71 sets → 7

---
## CVG Web Factory Launched — /web-design Sales Page + Instant Estimator + Sitewide Banner — 2026-08-10
**Site:** CVG (ceeveeglobal.com)
**Phase:** New revenue line — AI-built websites for small businesses

### What was done
Turned `CVG-Web-Factory-Manual.pdf` (the new "sell AI-built websites, keep clients on Care Plans" business plan) and a draft quote-wizard HTML file into a live page. `/web-design` now has the build packages, Care Plans, a portfolio card linking to mep.ceyload.com, a 4-step "how it works" strip, and an embedded instant-price estimator that actually submits leads — plus a sitewide promo banner (fuller version on the homepage, slim version on every blog post) pointing traffic at it.

### Why
The manual's marketing plan calls for exactly this: a `/web-design` hub as the "shop window" that converts the site's existing tutorial-reader traffic into paying leads, with the intake form as the main CTA. None of it existed yet — the estimator was a standalone draft file with a no-op submit button.

### Steps taken
1. Recolored the estimator's original indigo/amber palette to the site's actual live Neve customizer brand color (orange `#ff6302`) — confirmed via `wp option get theme_mods_neve` rather than guessing from old docs, which turned out to reference an unrelated blue palette.
2. Built 4 new WPCode PHP snippets: `cvg-web-design-page.php` ([cvg_web_design] — the sales page), `cvg-estimator.php` ([cvg_estimator] — the quote wizard, ported from the draft HTML), `cvg-quote-handler.php` (new `wp_cvg_quote_requests` table + `wp_ajax_cvg_submit_quote` handler + `wp_mail()` notification, same nonce/honeypot/rate-limit shape as ABT's proven contact form), `cvg-web-design-banner.php` (hooked to `wp_body_open`, shown on the front page + every blog post, excluded on `/web-design` itself).
3. Deployed via a new installer script (`wpcode-snippets/install-cvg-web-design.php`), then created the page (`/web-design`, `[cvg_web_design]`).
4. Hit two real bugs during verification, both now documented in the installer's docblock so they don't recur: (a) WPCode snippet code must NOT start with an opening `<?php` tag — it evals stored code directly, so a leading tag caused a silent parse failure with nothing in debug.log, and the shortcode/hooks just never registered; (b) the `frontend_cl` WPCode location only loads on genuine page views, not on `admin-ajax.php` requests — the quote handler's AJAX action needed the `everywhere` location instead, or lead submissions would 400 with no registered handler.
5. Verified end-to-end: a real POST to `admin-ajax.php` with a live nonce correctly inserted a row into `wp_cvg_quote_requests` with every field populated, then deleted that test row. `wp_mail()` itself is currently blocked by Zoho ("Unusual sending activity detected") — pre-existing account-level throttle, unrelated to this code; leads still land safely in the DB either way since the insert always happens before the mail attempt.
6. Wrote `image-gallery/web-design-page/image-prompts.md` — a Comic Dimu hero image prompt (Nano Banana, orange brand colors) plus instructions for a manual screenshot of mep.ceyload.com. Both `CVG_WD_HERO_IMG_URL`/`CVG_WD_PORTFOLIO_IMG_URL` constants default empty and the page shows a styled placeholder until Dimu drops the images in.

### Result
`ceeveeglobal.com/web-design` is live with packages, Care Plans, portfolio, and a working instant-estimate → lead-capture flow. The homepage and every blog post now carry a banner linking to it.

### Known follow-ups (not blocking, flagged for Dimu)
- Zoho SMTP is currently rejecting `wp_mail()` sends site-wide ("Unusual sending activity detected") — leads are captured safely in the DB regardless, but email notifications won't arrive until that clears or Zoho's dashboard is checked.
- Primary nav menu isn't assigned to any location (`nav_menu_locations[primary] = 0`) — pre-existing issue, unrelated to this work. A "Web Design" nav item won't show in the header until that's fixed separately.
- Hero + portfolio images still need generating/capturing — see `image-gallery/web-design-page/image-prompts.md`.

### Blog/video angle
"I built a lead-gen funnel for my own web design service — with Claude Code and a WordPress site that already had 47 pages of clutter."

### Screenshots to take
- [ ] The `/web-design` page hero + pricing sections
- [ ] The estimator wizard mid-flow (price updating live)
- [ ] The homepage banner and a blog-post banner side by side

---
## Whole Project Migrated to Optiplex — Dashboard Self-Hosted, Error Pipeline Automation Restored — 2026-08-06
**Site:** CVG / ABT / Both
**Phase:** Infrastructure — hosting migration

### What was done
Moved the entire project from Dimu's laptop to his always-on home server (`dim-OptiPlex-7060`, a Dell OptiPlex 7060 running Ubuntu, reachable as `ubuntu-optiplex` / `192.168.1.207` on the LAN), controlled remotely by Claude as an ongoing multi-project control agent. The Flask review dashboard now runs there as a persistent systemd service instead of being launched manually from the laptop, and the Error Library pipeline — which had silently stopped running when the project moved, because its trigger was Windows Task Scheduler — got an equivalent daily systemd timer so it resumed auto-publishing without Dimu needing to notice or intervene.

### Why
Dimu wanted this project (like several others — a Facebook content pipeline, a cat-story generator, a car-restoration prompt generator) controlled from a machine that's always on, so scheduled/background work doesn't depend on his laptop being powered on. Deliberately kept scoped to "hosted on the Optiplex only" — Dimu explicitly said not to connect this to Coolify beyond what's already needed to reach the two live WordPress sites.

### Steps taken
> Credential rule: never write actual passwords/keys in steps. Write: "saved in .env → VAR_NAME"
1. Copied the project to `~/projects/ceeveeglobal-site-creation/` on the Optiplex via a local tar (excluding `node_modules`, `.venv`, `__pycache__`, `.git` — regenerable/vendor content at every depth, which is what took `abt-products/` from 663MB down to a few MB of real files)
2. Froze the working Windows `.venv`'s exact package versions (`pip freeze`), stripped Windows-only packages (`pywin32`, `colorama` variants), and installed the rest into a fresh Python 3.12 venv on the server — imported cleanly first try
3. Changed the server's copy of `dashboard.py` to bind `0.0.0.0:5050` instead of `127.0.0.1` (the laptop's copy stays `127.0.0.1` — local-only there is still correct); created `cvg-dashboard.service` (systemd, `Restart=always`) and a `ufw` rule opening `5050/tcp` to the LAN (`192.168.1.0/24`) only
4. Generated a dedicated SSH keypair on the Optiplex and added it to `coolify-vps`'s `authorized_keys` so Claude sessions running on the server could reach the WordPress VPS directly — then, per Dimu's explicit instruction the same day, deleted that key from both ends again; this project is Optiplex-only for now, nothing added to Coolify
5. Confirmed the project is covered by the Optiplex's existing 4-hourly Google Drive backup (`rclone`, generic `~/projects` sweep, no project-specific config needed) and triggered one immediate run to get it backed up right away rather than waiting for the next cycle
6. Found the Error Library pipeline (`error-library/pipeline.py`, `ERROR_PIPELINE_PUBLISH_STATUS=publish` — auto-publishes live, no review gate) had no trigger anymore: its old mechanism was `error-library/scheduler.xml`, a Windows Task Scheduler job firing 3 minutes after every laptop login, which obviously doesn't exist on a headless Linux box. Built `cvg-error-pipeline.service` + `.timer` (daily, 04:00) as the equivalent
7. First timer test failed (`claude not found in PATH`) — systemd services don't inherit the interactive shell's PATH; fixed by wrapping `ExecStart` to export `~/.npm-global/bin` first. Re-verified with `--dry-run --limit 1`, which walked the full topic queue cleanly with zero errors
8. During this session the Optiplex itself dropped off the LAN twice (once for ~3.5 days — turned out to be the machine actually powered off, not just a network blip, confirmed via a kernel-log gap and `/proc/uptime`; once briefly, cause unconfirmed) — not fixable remotely each time, required Dimu to physically check the machine and reseat a USB wifi adapter

### Result
Dashboard live at `http://192.168.1.207:5050`, reachable from any device on the LAN, independent of the laptop. Error Library back to publishing on its original daily cadence, now via `cvg-error-pipeline.timer` instead of Windows Task Scheduler. Backups confirmed flowing to Google Drive automatically. No Coolify footprint beyond the two WordPress sites that were already there.

### Blog/video angle
"I moved my WordPress content pipeline off my laptop onto a $200 desktop running Linux — here's what actually broke (and what didn't)"

### Screenshots to take
- [ ] Dashboard open at `http://192.168.1.207:5050` from a phone/tablet on the same wifi, proving it's laptop-independent
- [ ] `systemctl list-timers` on the Optiplex showing `cvg-dashboard.service`, `cvg-error-pipeline.timer` alongside the other hosted projects

---
## Image Gallery Unification — No More Image-Gen APIs, Anywhere — 2026-07-21
**Site:** CVG
**Phase:** Content pipeline consolidation

### What was done
Dimu caught the newly-built Pinterest pin pipeline calling Gemini directly for image generation and shut it down project-wide: no script may call an image-generation API, ever — every image gets a written prompt + expected filename, and Dimu drops the actual file in by hand. In enforcing that on the pin pipeline, also consolidated three previously-separate manual-image folders (`blog-images/`, `error-library/image-queue/`, and the pin-staging folder) into one shared `image-gallery/<batch>/` convention, one subfolder per usage batch, each with its own `image-prompts.md`.

### Why
Dimu's rule, stated directly: image generation is his job, not a script's. This was already true for blog posts and error posts (neither ever called an image API) — only the brand-new pin pipeline broke the pattern, so the fix was to bring it back in line rather than special-case it.

### Steps taken
1. Rewrote `scripts/generate-pins.py` to drop all Gemini/google-genai code — it now writes `image-gallery/<slug>-pins/image-prompts.md` + one JSON metadata sidecar per pin and stops
2. Rewrote `scripts/watch-pins.py` to scan `image-gallery/*-pins/` and process each pin image as it individually appears (upload to MinIO, insert MongoDB doc, delete the image + sidecar + prompt row) rather than waiting for a generator-written "done" flag — no need to wait for the full set
3. Moved `upload-draft-images.py`'s expected image location from top-level `blog-images/` + `content-queue/drafts/<slug>/image-prompts.md` to a single `image-gallery/<slug>/` folder holding both; same row-removal-as-processed + folder cleanup pattern
4. Moved `error-library/pipeline.py`'s image-prompt writer and `process-images.py`'s scanner from a flat `error-library/image-queue/` + single shared `image-prompts.md` (keyed by a post_id column) to per-post `image-gallery/<post_id>/` folders — simplified the table to 3 columns since the folder itself now encodes the post_id
5. Updated `write-post.md` (Phases 6/7/9) to match the new location
6. Migrated the 2 real in-flight drafts with pending image prompts (`docker-pip-install-slow-fix`, `install-claude-code-connect-vps`) into their new `image-gallery/<slug>/` homes rather than losing them
7. Left `error-library/image-queue/Comic dimu.png` (the character reference image, not a queue file) exactly where it was — many scripts/docs hardcode that path
8. Tested for real: `generate-pins.py` end-to-end (Claude call, zero image-API calls, correct prompts.md), `watch-pins.py` against a synthetic drop (uploaded the one pin whose image existed, correctly left the other two waiting, then fully cleaned up the folder once all three arrived), `error-library/process-images.py --dry-run` against a synthetic post folder
9. Flagged (rather than silently patched) that `dashboard_app`'s `/api/draft/<slug>/generate-images` Flask route still called Gemini via `generate-images.py`/`generate-and-convert.py` — Dimu confirmed he wanted it fixed too. Converted it to copy the draft's prompts into `image-gallery/<slug>/image-prompts.md` and return a plain message instead of running a background Gemini job; updated the matching `triggerGenerateImages()` JS to show that message instead of streaming generation progress. (Turned out this button wasn't wired to any visible template element anyway — dead code today — but it's now correct if it's ever re-wired.) Verified `dashboard_app` still imports cleanly after removing the now-unused `GENERATE_CONVERT_SCRIPT`/`GENERATE_IMG_SCRIPT`/`CONVERT_WEBP_SCRIPT` imports from `routes.py`.

### Result
Zero image-generation API calls anywhere in the active pipeline. One consistent `image-gallery/<batch>/` convention across blog posts, error posts, and Pinterest pins — write prompt, wait for drop, process, clean up.

### Blog/video angle
"I told Claude to stop using AI image generation entirely — here's the folder convention it built instead"

### Screenshots to take
- [ ] `image-gallery/` root showing subfolders for a blog post, an error post, and a pin batch side by side

---
## Pinterest Pin Pipeline — Local Half Built (n8n Deferred) — 2026-07-20
**Site:** CVG
**Phase:** Content repurposing — new Pinterest distribution channel

### What was done
Built the local half of an automated Pinterest pin pipeline: every CVG post published via `scripts/publish-draft.py` now triggers `scripts/generate-pins.py`, which asks Claude for 3-4 distinct pin variants (title/description/board/image prompt) and generates each as a 1000×1500 image via Gemini ("Nano Banana"), staged locally. A second script, `scripts/watch-pins.py`, uploads completed sets to MinIO and writes one queue document per pin into a new MongoDB collection, cleaning up local files only once every pin is confirmed stored. The original brief specified a single all-in-n8n workflow; Dimu redesigned it mid-build into this local-generation-then-MongoDB-handoff shape, with the actual Pinterest-publishing side of n8n deliberately built later, against the MongoDB schema this pipeline produces.

### Why
Pinterest is a new distribution channel for CVG's tutorial content. Pinterest API access ("CVG Pin Automation" app, ID 1592245) is still pending Trial approval, so the local generation/staging/queueing side could be built and fully tested now, independent of that approval.

### Steps taken
> Credential rule: never write actual passwords/keys in steps. Write: "saved in .env → VAR_NAME"
1. Investigated the existing n8n instance (`n8n.aibuilttools.com`, read-only via its Postgres DB) before building anything — found no MinIO/Nano Banana/Pinterest credentials existed yet, no "Cats of Germany" workflow on this instance despite the original brief assuming one, and no public n8n API key configured (so nothing could be created there programmatically)
2. Dimu redesigned the architecture mid-conversation: local Python scripts handle generation → staging → MinIO/MongoDB handoff; n8n only picks up from MongoDB later, built separately
3. Confirmed the shared VPS MinIO (`s3.ceeveeglobal.com`) and MongoDB (exposed at `194.163.177.244:5432` via an nginx stream proxy) are both reachable from the local machine, not just from inside the VPS's Docker network
4. Created a new MinIO bucket `cvg-pinterest-pins` with a public-read policy (existing `ceeveeglobalimages` bucket left untouched)
5. Added `ANTHROPIC_API_KEY` (optional — falls back to the authenticated `claude` CLI, matching the existing `generate-draft-content.py` pattern), `GEMINI_API_KEY`-adjacent MinIO/Mongo vars to `.env` — saved in .env → `MINIO_ENDPOINT_URL`/`MINIO_ACCESS_KEY`/`MINIO_SECRET_KEY`/`MINIO_PINTEREST_BUCKET`/`MONGO_URI`/`MONGO_DB`
6. Built `scripts/generate-pins.py` (Claude pin-copy generation with JSON validation + one retry, Gemini image generation with a model-fallback chain, Pillow center-crop to exact 1000×1500) and `scripts/watch-pins.py` (MinIO upload + MongoDB insert + only-on-full-success cleanup, retry-safe on partial failure)
7. Wired the hook into `scripts/publish-draft.py`: fires after a real publish/schedule, never fails the parent publish if pin generation errors
8. Tested for real: pin-copy generation via Claude (both dry-run and full run) produced valid, distinct 3-pin JSON; installed `boto3`/`pymongo`/`truststore` into the project venv (worked around a corporate-proxy TLS issue using `truststore` instead of certifi); ran `watch-pins.py` against a synthetic pin set, confirmed real uploads to MinIO, real documents in MongoDB `cvg_pinterest.pins_queue`, the public image URL fetchable over HTTPS, and full folder cleanup — then deleted all test data from both systems
9. Hit an external blocker during real image-generation testing: this Gemini/Google AI Studio project has zero free-tier quota (`limit: 0`) for every current "Nano Banana" image model, and also discovered `scripts/generate-images.py`'s hardcoded model list is now stale (404s on this account) — both logged for Dimu, not something fixable from this side

### Result
Steps 1, 2, 4, 5, 6 of the redesigned pipeline are built and tested end-to-end; Step 3 (actual image bytes) is code-complete but blocked on Gemini billing/quota. The n8n side (Pinterest OAuth2 credential, poll/stagger/publish workflow) is intentionally not built yet — full documentation of what it needs to consume (the `pins_queue` schema, board list, credentials) is in CLAUDE.md.

### Blog/video angle
"I asked Claude to build a Pinterest automation pipeline, and it redesigned the architecture live — then hit a Google billing wall right at the finish line"

### Screenshots to take
- [ ] `content-queue/pending_pins/<slug>/` mid-generation with a real pin set
- [ ] A real pin document in MongoDB Compass / `pins_queue`
- [ ] The uploaded pin image served from `s3.ceeveeglobal.com`
