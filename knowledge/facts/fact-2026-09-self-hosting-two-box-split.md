---
id: fact-2026-09-self-hosting-two-box-split
type: fact
topics: [self-hosting, automation]
projects: [cvg]
source: repo-2026-09-content-machine
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: public
---
# "Run the whole stack on one $4 VPS" does not hold — it is two boxes, about $15

The self-hosting pitch is that the paid SaaS stack most small businesses rent —
managed WordPress, Mailchimp, SendGrid, analytics — is the same open-source
software they could run themselves. That part is true. The one-cheap-box version
of it is not, and the numbers say so:

- Coolify's own minimum is 2 cores / 2 GB **before you deploy anything**.
- Postal's published minimum is 2 cores / 4 GB, and its docs recommend 8 GB on
  its own, because it runs MariaDB *and* RabbitMQ beside its web and workers.
- Mautic 5 brings its own database and cron.

So WordPress + Matomo + n8n sit comfortably on a ~$5 box (Contabo Cloud VPS 10,
4 vCPU / 8 GB / 75 GB NVMe, verified 2026-09-24 as the plan that actually
matches the price claim — Hetzner's CX22 was retired in the June 2026 renaming).
Postal and Mautic want a second box, or a step up to 16 GB.

Call it **$15/month against $125** — still an 88% cut and $1,320 a year kept,
plus a customer list nobody else holds a copy of. The honest split is the
stronger version of the claim, because the overclaim is what the comments
argue with.
