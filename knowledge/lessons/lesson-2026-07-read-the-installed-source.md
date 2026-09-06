---
id: lesson-2026-07-read-the-installed-source
type: lesson
topics: [engineering-thinking, tools]
projects: [abt]
source: doc-2026-09-abt-engineering-journal
source_url: null
date: 2026-07
status: current
superseded_by: null
visibility: agents-only
---
# The installed version's source is the spec — read it before writing against the API

Before writing a line of the points-purchase mu-plugin, the actual installed EDD 3.6.8
source on the server was read directly, rather than working from general knowledge of
`edd_insert_payment()`'s argument shape. Two things came out of it that memory would have
gotten wrong.

The hook signature: `edd_purchase_link_end`'s exact arguments were verified by grepping
the real source, not assumed.

The bug that never ran: `edd_build_order()` accesses `$item['tax']` with no `isset()`
guard — unlike `$item['discount']`, which has one. A missing `tax` key would have thrown
a PHP warning and left `null` where EDD expects a float, on every single order. Adding
`'tax' => 0` to the cart-details array took one line and happened before the first test,
rather than after a crash.

The transferable part: for a dependency you're writing *orders and payments* against, the
documented argument shape is a summary of the source, and summaries drop the guards that
aren't there. Reading it first turns a production bug into a one-line precaution. The
complement to this is [[lesson-2026-07-abt-real-path-testing]] — read before, test after.
