---
id: fact-2026-09-docker-publishes-past-ufw
type: fact
topics: [self-hosting, security]
projects: [cvg]
source: repo-2026-09-content-machine
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: public
---
# You cannot close a published Docker port with ufw — I measured it three ways

Coolify serves its dashboard on port 8000 as a Docker published port
(`8080/tcp -> 0.0.0.0:8000`). The obvious instinct is to close it with ufw once
you are done registering. It does not work, and I tested it rather than
assuming, on Coolify 4.3.23 on a real box, 2026-09-24:

| ufw state | Result on `:8000` |
|---|---|
| inactive | `302` |
| active, 22/80/443 allowed, 8000 never mentioned | `302` |
| active, explicit `ufw deny 8000/tcp` | `302` |

Docker writes its own iptables rules and they sit in front of ufw's, so the
rule you just wrote never sees the packet. This is why the real advice is
"register the admin account immediately, because that page is open to anyone
who finds it" — not "lock it behind the firewall afterwards".

Closing it properly takes a rule in Docker's own `DOCKER-USER` chain, or
putting the dashboard behind the reverse proxy on a domain.

I originally wrote the opposite into a script — that ufw would block the
dashboard — and the measurement is what caught it.
