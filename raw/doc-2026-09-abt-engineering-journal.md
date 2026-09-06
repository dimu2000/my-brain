---
id: raw-doc-2026-09-abt-engineering-journal
kind: raw
source: doc-2026-09-abt-engineering-journal
source_url: null
date: 2026-07
visibility: agents-only
truncated: true
---
# ABT build log — engineering entries (2026-07-14 → 2026-07-15)

Archive of the source fed as `doc-2026-09-abt-engineering-journal` on 2026-09-06. No
public URL exists for this document; this archive plus the feed id is the provenance for
the notes that cite it (CLAUDE.md §5.2).

Written *about* the owner's work in the third person, not in his voice — do not mine this
file for voice samples or quote it as his words.

The excerpt ends mid-document at a "Phase 7: Growth Loops — 2026-07-14" heading with no
body; that entry is not represented in this archive or in any note.

---

## ABT Points System — "Buy with Points" for EDD Digital Products — 2026-07-15
**Site:** ABT
**Phase:** New capability — points economy extended beyond tool usage

### What was done
Built a new mu-plugin (`abt-points-purchase.php`) that lets any EDD digital product opt into being purchasable with ABT points, as an alternative to real-money PayPal checkout — the first time points can be spent on *keeping* something rather than *running* something.

### Why
Dimu is planning to list more one-time digital products (script automation systems, and more) beyond the single lead-magnet product that exists today, and wants points usable as payment for those too — asked before building, since this touches real EDD purchase/order logic and is a materially different kind of feature than anything built so far (spend-to-own vs. spend-to-run).

### Steps taken
1. **Researched before writing any code.** Two Explore passes: one confirmed no EDD template/hook customization exists yet anywhere in the repo (clean, additive build), and catalogued the exact reusable primitives already available (`abt_user_authenticated_and_has_balance()`, `abt_rate_limit_check()`, `abt_lock_acquire()`/`abt_lock_release()`, `abt_terminate_request()`, the `ABT_POINT_TYPE` constant) so the new feature reuses rather than reinvents; the other read EDD 3.6.8's actual installed plugin source on the server directly (not general knowledge) to confirm exact hook signatures and function behavior before writing a single line against them
2. **Opt-in per product** via a `_abt_points_price` postmeta field, set through a small metabox on the Download edit screen — no code change needed for every future product Dimu adds, he just sets a number
3. **Button placement**: `edd_purchase_link_end` (verified the exact hook + argument signature by grepping EDD's real installed source, not assumed) — renders *alongside* the existing real-money buy button, never replacing it. States: logged-out, insufficient-balance (shown with real balance + a link to get more points), already-owned (via `edd_has_user_purchased()`), or the buy button
4. **Found and fixed a real bug before it ever ran**, by reading EDD's actual order-building code (`edd_build_order()`) line by line rather than trusting the general `edd_insert_payment()` argument shape from memory: it accesses `$item['tax']` directly with no `isset()` guard (unlike `discount`, which has one) — a missing `tax` key would have thrown a PHP warning and left a `null` where EDD expects a float. Added `'tax' => 0` to the cart-details array before ever testing, rather than finding it via a crash
5. **AJAX handler** (`wp_ajax_abt_purchase_with_points`, no `nopriv` — matches the existing tool-dispatcher's auth-required convention): nonce (a new, separate action, since the existing nonce helper is hardcoded to tool-runs) → rate limit → product/ownership validation → lock → balance check → deduct-before-grant with refund-on-failure (same shape as the tool dispatcher) → creates a real EDD order via `edd_insert_payment()` (price $0, gateway `abt_points`, a payment note recording the points paid) → `edd_update_payment_status(..., 'complete')` to fire a genuine `edd_complete_purchase` → returns the real download file link directly in the response
6. **Tested end-to-end against the real live site**, not assumed correct: a temporary test product ($19 real price, opted into a 50-point price) and a temporary admin-role test user (needed to view the page past `site_maintenance_mode`'s gate). Verified: insufficient-balance gate renders correctly and the AJAX handler independently rejects the same case server-side (temporarily set the price to 1000 points to force this, rather than skip it); a real purchase at exactly matching balance (50=50) succeeded, deducted points to 0, and created a genuine EDD order — confirmed directly via the database: `status=complete`, `gateway=abt_points`, `total=0`, with the "Paid with 50 ABT points" note actually present; `edd_has_user_purchased()` flipped to true; the page re-render correctly showed the "already owned" state instead of the button; a forced second AJAX purchase attempt was correctly rejected server-side; checked the user's full MyCred transaction log afterward and confirmed only the expected 4 entries fired (welcome bonus, test grant, daily-login bonus, the purchase deduction) — no unintended referral-reward or other side effect
7. **Confirmed the real, existing lead-magnet product (`Free Email Verification API`) was never touched** — no `_abt_points_price` meta on it, its own $0 EDD flow unaffected. Test product, test order, and test user all removed afterward

### Result
Any EDD download can now be made points-purchasable by setting one number on its edit screen — verified working end-to-end on the real site, not just built. Ready for Dimu to price and enable it on the real products he adds going forward.

### Known, accepted trade-off (flagged, not solved here)
A $0-cash points purchase still counts as a qualifying "first completed purchase" for the existing referral-reward hook (50 pts each side) — consistent with "a purchase is a purchase," not a new problem this feature introduces (welcome + login bonuses can already fund tool usage for free), but worth knowing if it ever needs a real-money floor later.

### Blog/video angle
"Reading a payment plugin's actual source code before writing against its API — the one missing array key that would have silently broken every order, caught before it ever ran instead of after"

### Screenshots to take
- [ ] The "Buy with N points" button next to a real EDD purchase button
- [ ] A completed points-based order in the EDD admin orders list, showing gateway "abt_points" and $0 total

---
## ABT Points System — Phase 6, Waves 1/2/4: WP-Side Integration + GitHub Push — 2026-07-15
**Site:** ABT (+ `fastapi-live` repo)
**Phase:** STEP 10, Phase 6 — all 17 tools now live end-to-end

### What was done
Pushed the FastAPI backend changes to GitHub (separate from the Docker-image deploy already done), then built and deployed the WordPress-side half of the 11 remaining tools from Waves 1/2/4 — dispatcher configs, front-end pages, a shared-runner extension for checkbox/select fields — and real-tested every one of them through the live dispatcher. Found and fixed 3 genuine bugs along the way that only surfaced under real testing, not local review.

### Why
The FastAPI backend went live yesterday, but the tools were still unreachable from WordPress — no dispatcher config, no page, no way for a real user to hit them. This closes that gap. Dimu also asked to keep git in mind going forward, not just the Docker deploy — the two are separate mechanisms (git push here does *not* trigger a redeploy, confirmed yesterday) and both needed doing.

### Steps taken
1. **GitHub push, scoped deliberately.** `D:\fastapi-live`'s working tree still has real unrelated pending changes (Dockerfile reordering, a 273-line rewrite of `error_tools.py`, unused content in `prompts.py`) that were never reviewed this session. Staged only what's either newly authored (the 11 routers, `main.py`'s new imports, `requirements.txt`'s real additions) or structurally required for the repo to build standalone from a fresh clone (`llm_api.py`'s already-fixed `instructor.from_openai()` client, 3 previously-untracked files `main.py` already imports) — confirmed via grepping actual imports that the excluded `prompts.py` additions are dead code nothing depends on. `config.json`'s real secrets never staged
2. **Dispatcher configs added** — `CUSTOM_TOOL_ID` 7 through 17 in `abt-tool-definitions.php`, matching each router's real `@router.get`/`@router.post` decorator and field names (verified against the actual source, not assumed from memory)
3. **Shared tool-runner extended** (`abt-tool-runner.php`) with a `checkbox` field type (needed for .htaccess Generator's 6 boolean toggles) and associative-array `select` options (`'value' => 'Label'`, backward-compatible with the existing flat-array usage) so www-handling could show a real label instead of the raw enum value
4. **Built + deployed 11 page snippets and draft pages** via the same `wp eval-file` WPCode-DB-insert pattern as every prior wave
5. **Found and fixed 3 real bugs, all via actual live testing, not code review:**
   - **Bool params silently 422'd.** `abt_api_request()`'s GET path builds its query string with `http_build_query()`, which casts PHP `false` to an empty string — FastAPI/Pydantic rejects an empty string as a bool. Nobody had used a `bool`-type param before .htaccess Generator, so this was a real, previously-untested code path. Fixed by mapping bools to `'1'`/`'0'` before building the query
   - **Missing `wp_unslash()` corrupted quote-containing input.** WordPress backslash-escapes every `$_POST` value on boot (`wp_magic_quotes()`) and `abt_sanitize_and_validate_params()` never undid it. Caught because Serialized Search & Replace silently fell back to "plain text mode" on input that should have parsed as valid serialized PHP. The more serious version of this bug: wp-config Auditor's backend redacts secret *values* via a regex anchored on literal single quotes (`define('DB_PASSWORD', '...')`) before ever building the AI prompt — with the escaping bug, a real password's quotes would arrive as `\'`, the regex wouldn't match, and the real value could have reached the LLM unredacted. Fixed with `wp_unslash()` before sanitizing; re-tested with a real `DB_PASSWORD` value and confirmed the redaction now works correctly (no password-related finding surfaced in the response, meaning it never reached the model unredacted)
   - **libmagic can't reliably detect minimal `.docx` files.** Track-Changes Stripper rejected a genuinely valid Word document as "unsupported file type" — `finfo_file()` reported `application/octet-stream` for it inside this specific container, while the *same code path* correctly identified an equivalent `.xlsx` as its real OOXML MIME type. Since real user Word docs could hit the same false negative, added a structural fallback: when the MIME guess is inconclusive (`application/zip` or `application/octet-stream`) and the tool config declares an `ooxml_internal_marker` (e.g. `word/document.xml`), open the file as a zip and check that the marker actually exists inside it — a stronger, not weaker, check than trusting the magic-byte guess. Applied to both Track-Changes Stripper and (defensively) Spreadsheet Diff
6. **Re-verified all 11 tools end-to-end** through the real live dispatcher after each fix — a temporary admin-role test user (`abt_e2e_tester`, needed because ABT is currently in maintenance mode and the mu-plugin's exclusion list only covers `/wp-admin` and logged-in admins for the frontend page load itself; `admin-ajax.php` is already excluded regardless of role) logged in via real `wp-login.php` POST, real cookies + a real nonce extracted from a live page load, then every tool called via genuine `curl -F` multipart or JSON POST against `admin-ajax.php` — checkbox toggles, multi-file uploads (Spreadsheet Diff's two `.xlsx` files), single-file uploads, and text-only POSTs all confirmed working, balance ticking down correctly (225→153 across the paid calls), plus one deliberate negative case (uploading a `.csv` where a `.docx` was expected — correctly rejected). Test user and all temp files removed afterward on both the VPS and the container

### Result
All 17 ABT tools (Wave 0's 6 + Waves 1/2/4's 11) are now live and reachable end-to-end — dispatcher, page, and tested real API round-trip. Three real bugs caught by testing rather than assumed away, one of them genuinely security-relevant (secret redaction). Pages are still draft, pending Dimu's review before publishing + adding to the menu.

### Blog/video angle
"Three bugs that only exist when you actually test the real path: a boolean that became an empty string, a password redaction that quietly stopped working because of WordPress's own magic quotes, and a valid Word document rejected because of a blind spot in a 20-year-old file-detection library"

### Screenshots to take
- [ ] The .htaccess Generator's checkbox form and generated output
- [ ] wp-config Auditor correctly redacting a real password value
- [ ] The balance ticking down across the 11-tool test run

---
## ABT Points System — Tools Hub Page + Publish + Menu — 2026-07-15
**Site:** ABT
**Phase:** STEP 10, Phase 6 — tool store now navigable

### What was done
Built a `/tools/` hub page listing all 15 real tools grouped into 4 categories (Plugin & Performance, Security & Config, File Utilities, SEO & Content) with point cost shown per card, published all 16 real tool pages (15 tools + Account) that were sitting in draft, and added a single "Tools" item to the Primary Menu pointing at the hub — instead of a 15-item dropdown, which would have been unusable.

### Why
After building and testing all 17 tools, there was still no way for an actual visitor to find them — no index page, no menu entry, everything in draft. Asked Dimu how he wanted tool discovery to work (hub page vs. dropdown vs. leave in draft) rather than guessing at an IA decision that affects the whole site's navigation; he picked the hub page.

### Steps taken
1. Built `abt-tools-hub-page.php` (`[abt_tools_hub]` shortcode) — a card grid grouped by category, each card linking to its tool page, styled consistent with the existing tool-runner design system (same blue accent, same card/border language)
2. Deployed the snippet, created and published `/tools/`, and published all 15 tool pages + the Account page that were still sitting in draft since their original build
3. **Deliberately left WP Error Expert and WP Error Expert Enhanced in draft** — per Dimu's earlier instruction to pause both (kept live in the API, never exposed/linked)
4. Added one "Tools" item to the Primary Menu (positioned after Blog, before Contact) rather than a 15-entry dropdown
5. Verified via `wp eval` that the shortcode renders all 15 cards correctly and the menu item landed in the right place — real site verification, not just "should work"

### Result
The tool store is now internally complete and navigable — hub page, 16 published pages, one clean menu entry. Still gated behind `site_maintenance_mode` (unrelated toggle, untouched here), so none of this is publicly visible yet until Dimu turns maintenance mode off when he's ready to launch.

### Blog/video angle
"Building 17 AI tools is only half the job — nobody can find them without an index page and a menu link, and cramming 15 tools into a nav dropdown is its own kind of broken UX"

### Screenshots to take
- [ ] The /tools/ hub page card grid
- [ ] The new "Tools" item in the live Primary Menu

---
## ABT Points System — FastAPI Backend Deployed to Production — 2026-07-14
**Site:** ABT (shared FastAPI backend, `tools.ceeveeglobal.com`)
**Phase:** STEP 10 — all of Waves 0's fixes, 1, 2, and 4's new tools now live

### What was done
Discovered the real deployment mechanism for this service (it's not what either of us assumed), fixed a broken local dev environment along the way, then built, tested, and deployed a new Docker image carrying every FastAPI change made this session — all verified live on the real production domain afterward, not just assumed to have worked.

### Why
Dimu asked to "push this into live." What looked like a simple `git push` turned out not to be — this service was never actually wired to deploy from the `dimu2000/Python_fast_api` GitHub repo at all.

### Steps taken
1. **Found the real deployment mechanism before touching anything.** Confirmed directly in Coolify's own Postgres database that this application (`id=4`, "Fast Api") is configured with `build_pack: dockerimage` — meaning Coolify only ever pulls a pre-built image from `docker.aibuilttools.com/pythonfastapi:latest` (Coolify's own self-hosted registry) and restarts; it never builds from git. Confirmed this wasn't just theoretical by diffing file lists: `origin/main` doesn't even contain `email_tools.py`/`wp_analyzer.py`/several others that are unquestionably running in production today (tested directly). A `git push` alone would have done nothing
2. **Fixed your local venv while investigating a related error** — `D:\fastapi-live\venv` turned out to be a raw copy of a different, unrelated project's venv (`D:\Hasan\...\Python-Fast-API-main\venv`), so its `uvicorn`/`pip` executables silently ran against that other project's packages. Recreated it properly (old one preserved at `venv-old-broken`, not deleted) and reinstalled everything — including Pillow, PyPDF2, openpyxl, and python-docx, which were real gaps in `requirements.txt` this session had accumulated (installed locally for testing, never actually declared)
3. **Built the deploy image on the VPS, not blindly from the dirty working tree.** `D:\fastapi-live`'s git working tree had substantial unrelated pending changes never reviewed this session (a `Dockerfile` reordering that would hurt build caching, a 273-line rewrite of `error_tools.py`, changes to `prompts.py`/`blog_models.py`/`words_filter.py`) — none of these were included. Packaged only the actual source (excluding `venv`/`.git`/`log`), used git HEAD's original, known-good `Dockerfile` rather than the unreviewed modified one, and built on the VPS via the existing authenticated Docker registry login already present there
4. **`app/config.json` was never a concern for the image build itself** — confirmed `config_loader.py` reads it directly from disk with no environment-variable alternative, meaning the current production image already has real secrets baked in the same way (an existing pattern, not something introduced here). The only actual risk was git — config.json was never staged, committed, or pushed anywhere
5. **Tested the new image before it ever touched live traffic** — ran it as a separate, port-isolated container on the VPS first: confirmed a known-existing route still worked (no regression), confirmed several brand-new routes now existed and returned real results, and specifically re-verified the Performance Analyzer AI fix produced genuine varied recommendations (not the old static fallback) in this exact built image
6. **Cut over without going through Coolify's own redeploy button** (no working API token/webhook auth readily available) — captured the running container's exact configuration via `docker inspect` (network, all 42 Traefik/Coolify labels, restart policy, env vars), renamed the old container aside as a rollback rather than deleting it, and recreated it under the original name with the new image and byte-for-byte identical configuration, so Coolify's own tracking and Traefik's routing stay intact
7. **Verified against the real live domain afterward, not just the isolated test** — `https://tools.ceeveeglobal.com` health check, an existing route, all 11 new routes, and the Performance Analyzer AI fix all confirmed working on actual production traffic. Cleaned up the temporary build directory and tarball on the VPS (which briefly held the real `config.json` secrets during the build — never written to git, but cleaned up promptly regardless)

### Result
Every tool built this session — Waves 1, 2, and 4's new FastAPI routers, plus the Wave 0 AI fixes — is now live on `tools.ceeveeglobal.com`, verified against real production traffic. The old container remains stopped (not deleted) as `aogkg00o0sk4ss0o4c00c04o-121638627298-rollback` for a quick revert if anything surfaces later that testing didn't catch.

### Blog/video angle
"The deployment mystery: why 'just push to GitHub' didn't work, and what actually happens when you dig into a production system you didn't build yourself"

### Screenshots to take
- [ ] The new routes responding on the live tools.ceeveeglobal.com domain
- [ ] Coolify's container list showing the redeployed app

---
## ABT Points System — Phase 7: Growth Loops — 2026-07-14
