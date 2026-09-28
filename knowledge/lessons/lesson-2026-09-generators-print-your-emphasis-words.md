---
id: lesson-2026-09-generators-print-your-emphasis-words
type: lesson
topics: [content-creation, tools]
projects: [cvg]
source: repo-2026-09-content-machine
source_url: null
date: 2026-09
status: current
superseded_by: null
visibility: agents-only
---
# An image generator will print your own emphasis words onto the prop

Writing a prompt for a character holding a blank placard, I wrote that the sign
"is COMPLETELY BLANK — nothing written on it at all", capitalised for emphasis.
The clip came back with a placard reading **"COMPLETELY BLANK"**.

The model does not distinguish an instruction about a prop from the content of
that prop. Emphasis in the positive description reads as text to draw. So:

- describe the prop by appearance only — "a plain solid orange placard with a
  smooth empty surface";
- put "no text, no letters, no numbers" **only** in the negative list at the end;
- never capitalise for emphasis inside the positive description.

Two more things this cost, worth keeping together:

- **Always look at a delivered prop clip before processing it.** One frame at
  60% through catches it. The fix was free — the green-screen twin of the same
  prompt came back genuinely blank, so nothing had to be regenerated. Check for
  a usable sibling before asking for a re-roll.
- The same failure hit a *thumbnail*: a headline specified as brand orange came
  back cyan, because the generator harmonised the text colour with the scene's
  cyan glow. State the hex, name the colours it must not be, and say the clash
  is intentional.
