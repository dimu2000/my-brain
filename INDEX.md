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
- [project] CeeVeeGlobal.com — tutorial blog on AdSense + affiliates, now also the CVG Web Factory web-design service | visibility: agents-only | projects/ceeveeglobal.md
- [project] AIBuiltTools.com — EDD digital store + metered AI-tool platform on a points economy, built but still in coming-soon mode | visibility: agents-only | projects/aibuilttools.md
- [project] ABT (AIBuiltTools) — 17 AI tools for WordPress owners, paid for in points instead of cash | visibility: agents-only | projects/abt.md
- [lesson] Local tests passing meant nothing until every tool was re-run through the real path — three bugs that only existed on admin-ajax.php | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-abt-real-path-testing.md
- [lesson] A secret-redaction regex that fails open — WordPress magic quotes broke it silently and nearly sent a real DB_PASSWORD to the LLM | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-secret-redaction-magic-quotes.md
- [lesson] "Pushed to git" is not "deployed" — 11 tools shipped nothing because Coolify pulls a prebuilt image, never builds from git | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-deployed-not-deployed-coolify.md
- [lesson] The installed version's source is the spec — reading EDD's order builder first turned a production bug into a one-line precaution | status: current | visibility: agents-only | knowledge/lessons/lesson-2026-07-read-the-installed-source.md
