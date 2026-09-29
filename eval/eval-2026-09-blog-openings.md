# Eval run — 2026-09-29 — blog openings, mind-backed vs. plain

## ⚠ Not a real eval — a harness smoke test

The protocol in `eval/README.md` calls for **blind human scoring**: Dimuthu scores each
pair without knowing which is which, on Voice / Substance / Would-I-post-it. That did not
happen on this run — the pairs were shown for scoring and no scores came back before the
session moved on to reveal the key. What follows is Claude's own read of its own output,
which is not a blind judgment and should not be trusted as one. It confirms the harness
mechanics work (real tasks, real retrieval, blind generation, revealable key) but **settles
nothing about whether the mind actually helps.** Re-run this with real scores before drawing
any conclusion from it.

## Method

- **Tasks:** 10 real, unproduced topics — every entry in
  `content-machine/idea-bank.md` marked `Status: Raw` as of 2026-09-29. None invented.
- **Format:** opening (3–5 sentences) + a short structural outline, per version. Chosen
  over a full piece to keep the set to 10 tractable rather than cutting the task count to
  make full pieces affordable.
- **Mind-backed version:** written using `identity/core.md`, `identity/voice.md`,
  `identity/beliefs.md`, and the topic's matching `knowledge/` notes where one existed
  (e.g. task 3 pulled from `lesson-2026-08-host-bound-triggers-die-silently.md`, task 4
  from the human-approval belief).
- **Plain version:** written with no mind context, generic competent tech-blog voice.
- **Blinding:** each task's mind/plain order was randomized (seeded) before either version
  was written, and labels (A/B) were shown without the key. Full drafts and the key are in
  the session scratchpad, not committed (they're eval inputs, not knowledge).

## Claude's own read (not a substitute for real scoring)

Every "plain" version opened with a generic framing sentence ("X is important, but...") and
never used a first-person real number, date, or named failure. Every "mind" version opened
mid-problem, used a concrete date or number pulled from the actual source material (66/66,
2026-06-13 host move, ceeveeglobal.com by name), and closed the outline on something only
Dimu could write (a checklist he actually uses, a thing he'd tell a client). By the voice.md
rubric this is the expected shape of the difference — which is exactly why it isn't a safe
substitute for blind human scoring: the harness was built by the same process that wrote
voice.md, so it grading itself as compliant with voice.md is closer to a unit test than an
eval.

## What this run actually established

1. The retrieval → generation → blind-compare → reveal mechanism works end to end.
2. There are enough real, unproduced tasks (10, from idea-bank alone) to run a real eval
   without inventing prompts — 5 short of the ~15 the protocol suggests; no source of real
   reader questions/comments was available this run to close that gap.
3. Nothing here should be read as "the mind works" until a human scores it blind.

## Next run

Re-show the same 10 (or a fresh set) for real scoring, or do a handful (5–6) rather than all
10 if the full set is too much to sit through — partial real scoring beats complete
self-scoring. Consider pulling the missing 5 tasks from real reader questions if any exist
(comments, DMs) rather than idea-bank alone.
