# INDEX

One line per entity in the mind. This is the catalog agents read FIRST to
decide what to open — so a line should say enough to make that decision and
no more.

Format: `` - [type] one-line description | status: … | visibility: … | path ``

The feeder maintains this file; a serving layer regenerates a tier-filtered
version of it per consumer, so what you see here is the full private view.

## identity
- [identity] Dimu Harshana: self-taught tech educator turning learners into builders; CVG, ABT, AI-as-developer; AI works, I approve | status: current | visibility: agents-only | identity/core.md
- [identity] Buildable content, peer not guru, human approval gate, free/self-hosted over paid, code over page builders; 3 changes of mind | status: current | visibility: agents-only | identity/beliefs.md
- [identity] Short declarative peer voice, real title/sentence examples, Intro → Steps → Advanced Tips → Wrap-up, per-platform (blog/video/shorts/social) | status: current | visibility: agents-only | identity/voice.md

## projects
- [project] CVG Project — two WordPress sites (tutorial blog + product store) and one content machine | last-verified: 2026-09-22 | visibility: agents-only | projects/cvg.md
- [project] CeeVeeGlobal — WordPress solutions, tutorials and AI tools; mid-rebuild with Claude Code as the developer | last-verified: 2026-09-03 | visibility: agents-only | projects/ceeveeglobal.md
- [project] AIBuiltTools.com — EDD digital store + metered AI-tool platform on a points economy, built but still in coming-soon mode | last-verified: 2026-09-06 | visibility: agents-only | projects/aibuilttools.md
- [project] ABT (AIBuiltTools) — 17 AI tools for WordPress owners, paid for in points instead of cash | last-verified: 2026-09-06 | visibility: agents-only | projects/abt.md

## knowledge

### takes
- [take] Not a guru — a peer who's slightly ahead, never preaching from a mountaintop | status: current | visibility: agents-only | knowledge/takes/take-2026-09-not-a-guru-peer-slightly-ahead.md
- [take] Teach what I test — content has to be buildable, not just understandable | status: current | visibility: agents-only | knowledge/takes/take-2026-09-teach-what-i-test.md
- [take] Use Claude as an actual developer, not a chatbot — give it the files, the database and the error log | status: current | visibility: agents-only | knowledge/takes/take-2026-09-claude-as-developer-not-chatbot.md
- [take] One job per script, one thing per plugin — no monolithic scripts | status: current | visibility: agents-only | knowledge/takes/take-2026-09-modular-scripts-no-monoliths.md
- [take] Image-generation APIs: "not for now", not never — scripts still write the prompt and wait, until Dimu says to switch | status: current | visibility: agents-only | knowledge/takes/take-2026-09-image-apis-not-for-now.md
- [take] Code and content still split by risk, but staging is gone — content gated by Review Queue, code by Dimu's go-ahead per change | status: current | visibility: agents-only | knowledge/takes/take-2026-09-code-vs-content-risk-split.md
- [take] Code goes through staging; content goes straight to live — split the workflow by risk, not convenience | status: superseded | visibility: agents-only | knowledge/takes/take-2026-09-code-staging-content-live.md
- [take] No script may call an image-generation API — scripts write the prompt and wait for a hand-made image | status: superseded | visibility: agents-only | knowledge/takes/take-2026-09-no-image-generation-api-in-scripts.md

### stories
- [story] Why the CeeVeeGlobal rebuild started — years of snippets, an unused point system and a very slow site | status: current | visibility: agents-only | knowledge/stories/story-2026-09-ceeveeglobal-rebuild-origin.md

### lessons
- [lesson] Local tests passing meant nothing until every tool was re-run through the real path — three bugs that only existed on admin-ajax.php | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-abt-real-path-testing.md
- [lesson] A secret-redaction regex that fails open — WordPress magic quotes broke it silently and nearly sent a real DB_PASSWORD to the LLM | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-secret-redaction-magic-quotes.md
- [lesson] "Pushed to git" is not "deployed" — 11 tools shipped nothing because Coolify pulls a prebuilt image, never builds from git | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-deployed-not-deployed-coolify.md
- [lesson] The installed version's source is the spec — reading EDD's order builder first turned a production bug into a one-line precaution | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-read-the-installed-source.md
- [lesson] Moving hosts is how you find out which jobs stopped running — a Windows Task Scheduler trigger left an auto-publishing pipeline silently dead | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-08-host-bound-triggers-die-silently.md
- [lesson] Two silent WPCode traps — a leading `<?php` kills the eval, and `frontend_cl` doesn't load on admin-ajax | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-08-wpcode-snippet-gotchas.md
- [lesson] A backup plugin can look configured and still never prune — `updraft_delete_local=0` silently voids retention; 93 GB of backups for a 717 MB site | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-09-updraft-local-delete-pruning.md
- [lesson] No ungated publish path — the Error Library now creates WordPress drafts only; auto-publish retired 2026-09-20 | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-09-no-ungated-publish-path.md
- [lesson] An image generator prints your own emphasis words onto the prop — describe appearance, negatives at the end | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-09-generators-print-your-emphasis-words.md
- [lesson] Killing staging didn't remove the review gate — it moved into the dashboard Review Queue | status: superseded | visibility: agents-only | knowledge/lessons/lesson-2026-09-review-queue-replaced-staging.md

### facts
- [fact] Who I write for: beginners becoming builders — overwhelmed by tools, need a clear starting point | status: current | visibility: agents-only | knowledge/facts/fact-2026-09-audience-beginner-to-builder.md
- [fact] My default content framework and style traits — Intro → Steps → Advanced Tips → Wrap-up | status: current | visibility: agents-only | knowledge/facts/fact-2026-09-cvg-content-framework-and-style-traits.md
- [fact] The 6-step setup that gives Claude Code live access to CeeVeeGlobal — SSH, CLAUDE.md, MCP server, audit, staged changes | status: current | visibility: agents-only | knowledge/facts/fact-2026-09-cvg-claude-code-setup.md
- [fact] The CVG project runs from an Ubuntu Optiplex home server, not a laptop | status: current | visibility: agents-only | knowledge/facts/fact-2026-09-project-runs-on-optiplex-home-server.md
- [fact] No script in the pipeline calls an image-generation API — prompt + filename out, hand-dropped image in, one `image-gallery/<batch>/` convention | status: current | visibility: agents-only | knowledge/facts/fact-2026-07-no-image-generation-apis.md
- [fact] You cannot close a published Docker port with ufw — measured 302 three ways on Coolify 4.3.23 | status: current | visibility: public | knowledge/facts/fact-2026-09-docker-publishes-past-ufw.md
- [fact] "Whole stack on one $4 VPS" does not hold — two boxes, ~$15/mo against $125 | status: current | visibility: public | knowledge/facts/fact-2026-09-self-hosting-two-box-split.md

## lenses
- [lens] building-in-public — default scope for audience-facing content; topics build-in-public/open-source/tools/engineering-thinking, ceiling public | status: current | visibility: public | lenses/building-in-public.md

## content-catalog
- [catalog] YouTube (CeeVee Global) — 77 published videos, 2022-01 → 2026-08, with ids for `source:` | visibility: public | content-catalog/youtube.md

## raw
- [raw] CVG project contract (CLAUDE.md) — owner/stack summary, per-site tables, the 10 Golden Rules, dashboard and pin pipelines | visibility: agents-only | raw/repo-2026-09-cvg-project-contract.md
- [raw] CVG Digital Business Master Plan — pointer to the unpublished business plan | visibility: agents-only | raw/repo-2026-09-cvg-master-plan.md
- [raw] CVG build log — five dated entries: disk rescue, Web Factory launch, Optiplex migration, image-gen ban, Pinterest pipeline | visibility: agents-only | raw/doc-2026-09-cvg-infra-journal.md
- [raw] Origin section of JOURNEY.md — why the CeeVeeGlobal rebuild started, the 6-step setup, the timeline | visibility: agents-only | raw/doc-2026-09-cvg-origin-story.md
- [raw] Dimu Harshana voice & profile — the internal voice file every CVG content script loads | visibility: agents-only | raw/doc-2026-09-cvg-voice-profile.md
- [raw] ABT build log — engineering entries, 2026-07-14 → 2026-07-15 | visibility: agents-only | raw/doc-2026-09-abt-engineering-journal.md
- [raw] Content-machine contract (CLAUDE.md) — approval iron rule, blog → video → social, error-pipeline routing, no-image-API-for-now | visibility: agents-only | raw/repo-2026-09-content-machine-contract.md
- [raw] Content-machine working repo — the video-06 build: ufw/Docker measurement, VPS sizing, image-generation failures | visibility: agents-only | raw/repo-2026-09-content-machine.md
