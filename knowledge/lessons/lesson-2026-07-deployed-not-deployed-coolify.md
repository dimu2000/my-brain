---
id: lesson-2026-07-deployed-not-deployed-coolify
type: lesson
topics: [self-hosting, engineering-thinking, tools]
projects: [abt]
source: doc-2026-09-abt-engineering-journal
source_url: null
date: 2026-07
status: current
superseded_by: null
visibility: agents-only
---
# "Pushed to git" is not "deployed" — find the real mechanism before you assume one

The FastAPI backend was assumed to deploy from its GitHub repo. It never did. Confirmed
directly in Coolify's own Postgres database that the application (`id=4`, "Fast Api") is
configured with `build_pack: dockerimage` — Coolify only pulls a prebuilt image from the
self-hosted registry (`docker.aibuilttools.com/pythonfastapi:latest`) and restarts. It
never builds from git. A `git push` alone would have shipped nothing.

The cheap confirmation that this wasn't theoretical: diff the file lists. `origin/main`
doesn't even contain `email_tools.py` or `wp_analyzer.py`, which are unquestionably
running in production today. Git and deploy were two separate mechanisms here, and only
one of them moved code.

Actually deploying meant building the image on the VPS from the packaged source using git
HEAD's known-good `Dockerfile` (not the dirty working tree's unreviewed one), testing it
as a port-isolated container first, then recreating the live container with configuration
captured byte-for-byte from `docker inspect` — network, all 42 Traefik/Coolify labels,
restart policy, env — so Coolify's tracking and Traefik's routing stayed intact. The old
container was renamed aside as a rollback, not deleted.
