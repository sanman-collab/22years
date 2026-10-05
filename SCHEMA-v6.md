# content.json v6 — schema for the thread rebuild

Every agent writes against this. Do not invent fields; if something is missing,
say so in your report rather than adding it.

## What changed and why

v5 showed a question, then a separate reveal page with four stacked cards in a
fixed order: *what happens* → *cost* → *twist* → *the lesson* → *your choice
analysis*. Five user-test sessions independently reported that reveals felt
generic and disconnected from the choice made. The cause is structural: 80 of
the reveal strings are shared across all options, only 59 are per-option, and
the per-option one rendered **last**.

v6 is a single downward-scrolling thread. Earlier decisions stay visible above,
so a reader can look back at what they chose. Each answer produces three cards,
in this order:

1. **Your choice** — per option. Opens by naming what they just did, then its
   consequence. This is `opt.<id>.result`.
2. **What happens** — shared per decision. The field evidence. `reveal.fact`
   plus `reveal.cost`.
3. **What this teaches / the roads not taken** — shared per decision. The
   twist and the lesson merged into one card, closing with one or two lines on
   where the other options would have led. `reveal.lesson` plus
   `reveal.alternatives`.

## Field reference

```jsonc
{
  "decisions": {
    "<id>": {
      "section": "WEEK 3 · THE DIG",       // phase, not topic. States when.
      "lead": "The pit is dug and the shuttering is up.",
      //  NEW, required. One or two sentences of scene before the question.
      //  Answers "where am I in the build?" Testers hit every question cold.
      "asker": "Your maistry asks",
      //  NEW, required. Who is putting this question to the player. Varies by
      //  seat — see persona.askers below. Plain, no title case.
      "prompt": "How deep do we dig?",
      //  Shorter than v5. The context has moved to lead/asker.
      "visual": "openings",                 // optional, see VISUALS.md
      "options": [
        {
          "id": "covers",
          "icon": "🔲",
          "label": "Two covers, 2×2 ft, at ground level",
          "note": "Costs more now. A hose can reach in later.",
          //  NEW, optional. One line of trade-off on the option card itself.
          //  Asked for in 3 of 5 sessions. Keep under 60 characters.
          "fx": { "A": 2, "M": 2, "E": 2 },
          "result": "You left two covers...",
          //  NEW, required. Was reveal.analysis.<id>. Must OPEN by naming what
          //  they chose in plain words, then give the consequence. This is the
          //  first thing they read after choosing, so it carries the
          //  acknowledgment the testers asked for.
          "result_by_role": {               // optional, see role voicing
            "contractor": "You told your man to..."
          }
        }
      ],
      "reveal": {
        "fact": "...",                      // unchanged
        "cost": "...",                      // unchanged
        "lesson": "...",                    // twist folded in; see below
        "alternatives": "The other two routes...",
        //  NEW, required. One or two lines: where the options they did NOT
        //  choose would have led. Brief. Do not re-describe every option.
        "fact_by_role": { },                // optional
        "lesson_by_role": { }               // optional
      }
    }
  }
}
```

### Removed from v5

- `reveal.twist` — merged into `reveal.lesson`. Keep the surprise, lose the
  separate card. The twist was the best-performing content in testing; it must
  survive the merge, so put it **first** inside `lesson`.
- `reveal.analysis` — becomes `options[].result`.

### Persona additions

```jsonc
"personas": [{
  "id": "contractor",
  "name": "Contractor",
  "icon": "🧾",
  "tagline": "You price it and deliver it.",
  "seat": "...",                            // unchanged, shown on results
  "motivation": "You won this job on a number...",
  //  NEW, required. Shown on the seat-select screen BEFORE committing.
  //  What this person wants and what pressure they are under. 2-3 sentences.
  //  Asked for explicitly in the contractor session.
  "scene": "It is March. The foundation is in...",
  //  NEW, required. Scene-setting shown once after picking the seat, before
  //  the first question. Establishes where this role enters the build.
  "askers": { "tanktype": "The owner asks", "depth": "Your own call" }
  //  NEW, required. Per-decision override of decision.asker, because the same
  //  question comes from a different direction depending on the seat. Falls
  //  back to decision.asker when a key is absent.
}]
```

### Config additions

```jsonc
"config": {
  "randomiseOptions": true,
  //  NEW. Shuffle option order per playthrough. The strongest option was last
  //  in 19 of 20 decisions, and a tester admitted picking last-because-last.
  //  The engine logs the shown position of the chosen option as pos_<id>.
  "showThread": true,
  //  NEW. The downward thread. false falls back to one screen at a time.
  "params": { }   // unchanged, but see below
}
```

`params.<k>.label` and `.help` are now **required** to be self-explanatory
without the help text — "1 out of 4 on axis" was reported as meaningless in
three sessions. Add `params.<k>.scale`: a short phrase naming what low and
high mean, e.g. `"0 = someone must climb in · 4 = a hose reaches from above"`.

## Voicing rules

Four decisions are seen by more than one seat AND read differently depending
on who you are. These need `result_by_role` / `lesson_by_role` entries:

| Decision | Seats | Why it differs |
|---|---|---|
| `labour_in_pit` | contractor, maistry | Contractor decides policy and cost; maistry sends a specific man he knows down a hole |
| `speak_up` | contractor, maistry | Contractor risks a client relationship; maistry risks his next job and has least standing |
| `record` | contractor, maistry, engineer | Engineer's duty is documentation; maistry's is memory; contractor's is liability |
| `slab_call` | maistry only | Already single-seat — but its text currently appears in contractor-visible `speak_up` reveals. Untangle |

Everything else: write role-neutral. Address the reader as the person in that
seat without naming another trade's concerns.

**Verified bug to fix:** `labour_in_pit` and `speak_up` reveals are currently
written in the maistry's voice and shown to the contractor. A tester caught
this from inside the contractor seat.

## House style — non-negotiable, this was called out

A tester flagged the writing as sounding machine-made. Specifically:

- **Em dashes.** v5 overuses them badly. Use a full stop or a comma. At most
  one em dash per card, and only where it genuinely marks an interruption.
- **"It's not just X, it's Y."** Banned. Also banned: "isn't about X, it's
  about Y", "not because X — because Y". v5 leans on this constantly.
- **Triads.** "Cheaper, faster, and it produces no document at all." Two items
  or four, not three-in-a-row as a rhythm.
- **Sentence fragments for drama.** "Every time. Forever." Use sparingly.
- Write the way the trade speaks. Short sentences. Concrete nouns. The sting
  comes from a plain fact stated plainly, not from the sentence shape.

Keep the tone unsentimental. This is about people who die in septic tanks; do
not make it more emotional than the facts already are.

## Jargon that must be explained on first use or removed

These appear in option labels, read at speed with no explanation. Confirmed by
audit and flagged by testers:

`precast` · `baffle wall` · `flush slab` · `parapet` · `cowl` · `soak pit` ·
`dispersion trench` · `invert levels` · `desludging` · `harness` ·
`backfilling` · `spec` · `front elevation` · `effluent`

"effluent" and "flush slab" were the two that actually blocked comprehension.
Replace in labels; define in the `lead` or the `result` if the concept matters.

## Known content errors to fix

- `effluent` reveal: "anywhere sane to go" — should be "safe".
- `vent` analysis for `proper`: "With the cowl, so it doesn't block" is an
  incomplete sentence.
- Anything implying septic gases are uniformly heavier than air. **Hydrogen
  sulphide is heavier than air; methane is lighter.** Both matter and they
  behave differently. Do not simplify this into something false.

## Output

Write **only** your assigned file in `rebuild/`. Do not edit `content.json` —
it is merged centrally after all agents report. Valid JSON, UTF-8, no comments
in the actual output.
