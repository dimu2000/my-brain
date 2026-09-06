---
id: fact-2026-09-project-runs-on-optiplex-home-server
type: fact
topics: [self-hosting, automation]
projects: [cvg]
source: repo-2026-09-cvg-project-contract
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---

# The CVG project runs from an Optiplex home server, not a laptop

Since 2026-08-06 the whole project lives on Dimu's Ubuntu Optiplex home server; agent sessions run
on that machine and drive the project directly. The laptop is no longer the machine that runs
things.

- The Flask dashboard — the canonical content engine — runs persistently as a systemd service with
  `Restart=always`, firewalled to the LAN. It is reachable from the laptop or phone over the local
  network, not from the open internet.
- The Error Library pipeline runs as a systemd service plus timer, daily at 04:00. It replaced a
  Windows Task Scheduler job that simply stopped firing once the laptop wasn't the host.
- Backups are covered by the Optiplex's generic 4-hourly `rclone` sweep of `~/projects` to Google
  Drive; no project-specific backup config exists or is needed.
- The two live WordPress sites still run on a Coolify VPS. Passwordless SSH and Docker access from
  the Optiplex to that VPS was verified working on 2026-09-02.

Source doc: `raw/repo-2026-09-cvg-project-contract.md`
