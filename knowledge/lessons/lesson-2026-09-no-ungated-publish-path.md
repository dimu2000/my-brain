---
id: lesson-2026-09-no-ungated-publish-path
type: lesson
topics: [automation, content-creation, engineering-thinking]
projects: [cvg]
source: repo-2026-09-content-machine-contract
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---

# No ungated publish path — the Error Library now drafts only

The review gate that moved into the dashboard Review Queue when staging was killed
([[lesson-2026-09-review-queue-replaced-staging]]) now covers the Error Library too. It was
the one pipeline that used to auto-publish live.

Both of its paths — the scraped daily run (`cvg-error-pipeline.timer`, 04:00) and real errors
Dimuthu logs via `report-error` — create a WordPress **draft**, never a live post, and queue a
row in `OUTPUTS/error-pipeline/QUEUE.md`. Going live is a manual step: open the draft in
wp-admin and press Publish.

The Notion-checkbox auto-publish watcher was retired 2026-09-20. The contract's reason: an
ungated publish path was the exact bug that got this pipeline moved out of ceeveeglobal in the
first place. Agents must never build or resurrect an auto-publish path for it without being asked.

Source doc: `raw/repo-2026-09-content-machine-contract.md`
