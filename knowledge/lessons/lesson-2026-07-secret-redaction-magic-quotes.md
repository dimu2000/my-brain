---
id: lesson-2026-07-secret-redaction-magic-quotes
type: lesson
topics: [security, llm-engineering, engineering-thinking]
projects: [abt]
source: doc-2026-09-abt-engineering-journal
source_url: null
date: 2026-07
status: current
superseded_by: null
visibility: agents-only
---
# A secret-redaction regex that fails open, and nothing errors to tell you

wp-config Auditor strips secret *values* before building the AI prompt, with a regex
anchored on literal single quotes — `define('DB_PASSWORD', '...')`. WordPress
backslash-escapes every `$_POST` value on boot (`wp_magic_quotes()`), and
`abt_sanitize_and_validate_params()` never undid it, so a real password's quotes arrived
as `\'`, the regex didn't match, and the unredacted value could have reached the LLM.

The failure mode is what makes this worth keeping: the tool kept working. Nothing threw,
nothing 500'd, the response still looked like an audit. It was caught sideways — a
different tool (Serialized Search & Replace) silently fell back to "plain text mode" on
input that should have parsed as valid serialized PHP, which is what exposed the shared
escaping bug underneath both.

Fixed with `wp_unslash()` before sanitizing, then re-tested with a real `DB_PASSWORD`
value and confirmed no password-related finding surfaced in the response — meaning it
never reached the model unredacted.

Generalized: a redaction pass that pattern-matches on exact characters degrades to "send
everything" rather than to an error. It needs a positive test with a real secret; the
absence of a crash proves nothing. Found via [[lesson-2026-07-abt-real-path-testing]].
