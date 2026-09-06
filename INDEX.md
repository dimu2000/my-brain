# INDEX

One line per entity in the mind. This is the catalog agents read FIRST to
decide what to open — so a line should say enough to make that decision and
no more.

Format: `` `id` — one-line description [status] [visibility] ``

The feeder maintains this file; a serving layer regenerates a tier-filtered
version of it per consumer, so what you see here is the full private view.

## identity
- `identity-core` — who I am, what I'm working toward [current] [agents-only]
- `identity-beliefs` — cross-cutting positions [current] [agents-only]
- `identity-voice` — how I write; agent instruction manual [current] [agents-only]

## projects
- `project-example-project` — delete me; a sample card so a fresh clone indexes [current] [public]

## knowledge
_Empty. Feed your first source and the feeder adds lines here._

## lenses
- `lens-building-in-public` — default scope for audience-facing content [current] [public]

## content-catalog
_Empty. Add a row per platform once you start cataloguing what you publish._
- [project] CeeVeeGlobal (CVG) — WordPress tutorial site plus the scripted pipeline that writes it, and now an AI-built-websites offer | visibility: agents-only | projects/ceeveeglobal.md
- [project] AIBuiltTools.com — EDD digital store + metered AI-tool platform on a points economy, built but still in coming-soon mode | visibility: agents-only | projects/aibuilttools.md
- [project] ABT (AIBuiltTools) — 17 AI tools for WordPress owners, paid for in points instead of cash | visibility: agents-only | projects/abt.md
- [lesson] Local tests passing meant nothing until every tool was re-run through the real path — three bugs that only existed on admin-ajax.php | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-abt-real-path-testing.md
- [lesson] A secret-redaction regex that fails open — WordPress magic quotes broke it silently and nearly sent a real DB_PASSWORD to the LLM | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-secret-redaction-magic-quotes.md
- [lesson] "Pushed to git" is not "deployed" — 11 tools shipped nothing because Coolify pulls a prebuilt image, never builds from git | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-deployed-not-deployed-coolify.md
- [lesson] The installed version's source is the spec — reading EDD's order builder first turned a production bug into a one-line precaution | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-read-the-installed-source.md
- [lesson] A backup plugin can look configured and still never prune — `updraft_delete_local=0` silently voids retention; 93 GB of backups for a 717 MB site | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-09-updraft-local-delete-pruning.md
- [lesson] Moving hosts is how you find out which jobs stopped running — a Windows Task Scheduler trigger left an auto-publishing pipeline silently dead | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-08-host-bound-triggers-die-silently.md
- [lesson] Two silent WPCode traps — a leading `<?php` kills the eval, and `frontend_cl` doesn't load on admin-ajax | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-08-wpcode-snippet-gotchas.md
- [fact] No script in the pipeline calls an image-generation API — prompt + filename out, hand-dropped image in, one `image-gallery/<batch>/` convention | status: current | visibility: agents-only | knowledge/facts/fact-2026-07-no-image-generation-apis.md
- [raw] CVG build log — five dated entries: disk rescue, Web Factory launch, Optiplex migration, image-gen ban, Pinterest pipeline | visibility: agents-only | raw/doc-2026-09-cvg-infra-journal.md
- [project] CVG Project — two WordPress sites (tutorial blog + product store) and one content machine | visibility: agents-only | projects/cvg.md
- [take] No script may call an image-generation API — scripts write the prompt and wait for a hand-made image | status: current | visibility: agents-only | knowledge/takes/take-2026-09-no-image-generation-api-in-scripts.md
- [take] One job per script, one thing per plugin — no monolithic scripts | status: current | visibility: agents-only | knowledge/takes/take-2026-09-modular-scripts-no-monoliths.md
- [lesson] Killing staging didn't remove the review gate — it moved into the dashboard Review Queue | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-09-review-queue-replaced-staging.md
- [fact] The CVG project runs from an Ubuntu Optiplex home server, not a laptop | status: current | visibility: agents-only | knowledge/facts/fact-2026-09-project-runs-on-optiplex-home-server.md
