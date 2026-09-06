---
id: lesson-2026-07-abt-real-path-testing
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
# Local tests passing meant nothing until every tool was re-run through the real path

Eleven tools passed local review and were wired into the dispatcher. Three real bugs
surfaced only once each one was called the way an actual user calls it: a logged-in test
user, real cookies, a nonce pulled from a live page load, and genuine `curl` multipart or
JSON POSTs against `admin-ajax.php`.

- **A bool became an empty string.** `abt_api_request()`'s GET path built its query with
  `http_build_query()`, which casts PHP `false` to `''`; FastAPI/Pydantic rejects an empty
  string as a bool, so the call silently 422'd. No `bool` param had ever been used before
  the .htaccess Generator's toggles — a genuinely untested code path, not a regression.
- **WordPress's own escaping corrupted input.** A missing `wp_unslash()`; see
  [[lesson-2026-07-secret-redaction-magic-quotes]] for the serious half of this one.
- **libmagic misread a valid `.docx`.** `finfo_file()` returned `application/octet-stream`
  for a minimal Word file while the same code path correctly identified an equivalent
  `.xlsx`. Fixed with a structural fallback: when the MIME guess is inconclusive, open the
  file as a zip and check the declared `ooxml_internal_marker` (`word/document.xml`) is
  really inside it — a stronger check than trusting the magic-byte guess, not a weaker one.

All 11 were re-run end-to-end after each fix, balance ticking 225→153 across the paid
calls, plus one deliberate negative case (a `.csv` uploaded where a `.docx` was expected,
correctly rejected). Related: [[lesson-2026-07-read-the-installed-source]].
