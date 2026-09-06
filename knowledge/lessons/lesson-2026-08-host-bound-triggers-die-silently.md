---
id: lesson-2026-08-host-bound-triggers-die-silently
type: lesson
topics: [automation, self-hosting]
projects: [ceeveeglobal]
source: doc-2026-09-cvg-infra-journal
source_url: null
date: 2026-08
status: current
superseded_by: null
visibility: agents-only
---
# Moving hosts is how you find out which jobs stopped running

Moving the whole CVG project off the laptop onto an always-on Dell OptiPlex
7060 was supposed to be a hosting change. It surfaced a dead pipeline: the
Error Library — which auto-publishes live, with no review gate — had silently
stopped. Its trigger was a Windows Task Scheduler job firing 3 minutes after
every laptop login. On a headless Linux box that trigger simply doesn't exist.
Nothing errored, nothing logged; the job just never ran again.

The replacement is a systemd service plus a daily 04:00 timer. Its first test
failed with `claude not found in PATH` — systemd services don't inherit the
interactive shell's PATH — fixed by exporting `~/.npm-global/bin` in
`ExecStart`.

Transferable: the trigger is part of the automation, and it's the part that
lives outside the repo, so it doesn't get copied with it. A job that stops
firing produces no error and no output, which makes it invisible until
someone goes looking for output that isn't there. Migration checklists should
enumerate triggers per host, not just code and data.

The whole point of the move was removing that dependency: scheduled and
background work shouldn't require a laptop to be powered on.
