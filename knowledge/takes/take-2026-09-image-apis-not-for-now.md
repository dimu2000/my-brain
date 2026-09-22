---
id: take-2026-09-image-apis-not-for-now
type: take
topics: [automation, content-creation]
projects: [cvg]
source: repo-2026-09-content-machine-contract
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---

# Image-generation APIs: not for now, not never

Scripts still don't call an image-generation API. A script's job stops at a complete,
copy-pasteable prompt and the expected filename; Dimu generates the image and drops it in.

What changed is the status of the rule. It is a current choice, not a permanent ban:

> VERBATIM: "I am not using gemini api key for now... later I will use API"

Said 2026-09-22. When he says to switch, generation goes through the API — `GEMINI_API_KEY` is
already set and `video-production/tools/gen_thumbnail.py` drives it. Until then, prompts and
filenames only.

Supersedes [[take-2026-09-no-image-generation-api-in-scripts]]. Origin of the rule:
[[fact-2026-07-no-image-generation-apis]]. Source doc: `raw/repo-2026-09-content-machine-contract.md`.
