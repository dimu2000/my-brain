---
id: lesson-2026-09-updraft-local-delete-pruning
type: lesson
topics: [wordpress, self-hosting]
projects: [ceeveeglobal]
source: doc-2026-09-cvg-infra-journal
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---
# A backup plugin can look configured and still never prune

CVG's VPS was at 76% disk and losing ~1 GB/day. UpdraftPlus had kept every
daily backup since 28 May 2026 — 71 sets, 1,514 files, 93 GB — for a site
whose uploads folder is 717 MB. The backups were 130× the size of the thing
they backed up, and at that rate the disk would have filled in roughly 7
weeks, taking down all 35 containers on the box, not just the blog.

`updraft_retain` was already `1`, so at a glance the retention looked
configured. The cause was a second option: `updraft_delete_local` was `0`.
With no remote storage configured, that makes UpdraftPlus skip retention
pruning entirely — the retention number is never consulted. It had been
ignored for 96 days. The only signal was one line in the plugin's log, on
every run:

> `Prune old backups from local store: nothing to do, since the user disabled local deletion and we are using local backups`

Transferable: when a setting looks correct but the behaviour it controls never
happens, read the tool's own log before touching the setting again — a second
option is often gating the first. Second trap in the same fix:
`UpdraftPlus_Backup_History::rebuild()` only *adds* sets it finds on disk and
never drops entries whose files are gone, so the plugin's history had to be
pruned separately to match reality.
