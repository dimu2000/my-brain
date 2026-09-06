---
id: fact-2026-07-no-image-generation-apis
type: fact
topics: [automation, content-creation]
projects: [ceeveeglobal]
source: doc-2026-09-cvg-infra-journal
source_url: null
date: 2026-07
status: current
superseded_by: null
visibility: agents-only
---
# Standing rule: no script in the pipeline calls an image-generation API

Since 2026-07-21, no script anywhere in the CVG pipeline may call an
image-generation API. Every image step writes a prompt plus the expected
filename and stops; Dimu generates and drops the actual file in by hand.
Stated as his rule: image generation is his job, not a script's.

The trigger was a newly-built Pinterest pin pipeline caught calling Gemini
directly. Blog posts and error posts had never called an image API, so the
fix was to bring the new pipeline back in line rather than special-case it —
including a dashboard route that still kicked off a background Gemini job,
now converted to write prompts and return a message.

Enforcing it produced one convention: `image-gallery/<batch>/`, one subfolder
per usage batch, each with its own `image-prompts.md`. It replaced three
separate manual-image folders (`blog-images/`, `error-library/image-queue/`,
and the pin-staging folder). The loop everywhere is the same: write prompt →
wait for the drop → process each image as it appears → clean up.
