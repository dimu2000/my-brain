---
id: lesson-2026-08-wpcode-snippet-gotchas
type: lesson
topics: [wordpress, tools]
projects: [ceeveeglobal]
source: doc-2026-09-cvg-infra-journal
source_url: null
date: 2026-08
status: current
superseded_by: null
visibility: agents-only
---
# Two silent WPCode traps, both about how it runs your code

Shipping CVG's `/web-design` page as WPCode PHP snippets hit two failures,
neither of which produced an error anywhere:

1. A snippet's code must **not** start with an opening `<?php` tag. WPCode
   evals the stored code directly rather than including a file, so a leading
   tag is a parse failure — nothing in `debug.log`, and the shortcodes and
   hooks simply never register.
2. The `frontend_cl` location only loads on genuine page views, not on
   `admin-ajax.php` requests. An AJAX handler registered there doesn't exist
   when the request actually arrives, so lead submissions 400 with no
   registered handler. Anything AJAX needs the `everywhere` location.

Both were caught by verifying end to end — a real POST with a live nonce,
then checking the row had actually landed in `wp_cvg_quote_requests` — not by
reading the code.

Transferable: a platform that *evals* your code instead of including it
changes what "valid PHP" means, and a location/scope setting that sounds
cosmetic actually decides whether your handler exists at all. Related design
choice from the same build: the lead insert runs before the mail attempt, so
leads keep landing in the database while `wp_mail()` is blocked upstream.
