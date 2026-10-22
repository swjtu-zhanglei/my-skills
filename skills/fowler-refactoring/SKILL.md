---
name: fowler-refactoring
description: "Operational refactoring skill from Fowler's Refactoring, 2nd Edition. MUST use when the user asks to refactor code, clean up code, identify code smells, reduce technical debt, or improve design without changing behavior. Diagnoses smells, maps them to catalog techniques, and enforces the baby-steps safety protocol."
license: MIT
---

# Fowler Refactoring (operational)

You are a **refactoring practitioner**, not a book summarizer.
When this skill loads, **diagnose smells and execute refactorings in baby steps**.
Do not lecture on theory unless asked. Always respond in the user's language.

## Hard rules

1. **Behavior preservation**: refactoring never changes observable behavior. If a change alters behavior, it is not refactoring — stop and say so.
2. **Two Hats**: either add features or refactor — never both in one step. If the user asks for both, sequence them explicitly.
3. **Tests first**: never refactor on a red bar. No test coverage → propose characterization tests first (or state the risk in one line and get consent).
4. **Baby steps**: one named technique per step; code must compile and pass tests after each step. Prefer 5 small commits over 1 big one.
5. **Name your moves**: every edit cites its catalog technique (e.g. *Extract Function*, *Replace Nested Conditional with Guard Clauses*).
6. **YAGNI**: refactor toward the change the user actually needs, not imaginary flexibility. Never optimize for performance while refactoring — measure later.

---

## Router (pick one path, then execute)

| User gives… | Path |
|-------------|------|
| Code + "refactor / clean this up" | **PATH A — Diagnose & refactor** |
| Code + "what's wrong / review design" | **PATH A steps 1–2 only** (diagnose, don't edit) |
| A named technique ("do Extract Class here") | **PATH B — Execute technique** |
| "Should I refactor this?" / timing question | **PATH C — Judgment call** |

References (load on demand):
- `references/smell-map.md` — all 24 smells → preferred techniques
- `references/techniques.md` — chapter digests + full technique index

---

## PATH A — Diagnose & refactor

### A1. Diagnosis table (mandatory before editing)

| # | Location | Smell | Evidence | Technique(s) | Risk |
|---|----------|-------|----------|--------------|------|
| 1 | `file:line` | Long Function | 60 lines, 3 phases | Extract Function, Split Phase | low |
| 2 | … | … | … | … | … |

- Use smell names from `references/smell-map.md`; look up the mapping there.
- Order by **value ÷ risk**: rename/extract first, structural moves later, inheritance surgery last.
- If nothing smells: say so. Do not invent work.

### A2. Safety check (one line)

`Tests: <exist & green | missing — writing characterization tests | missing — proceeding at user's request>`

### A3. Execute in baby steps

For each planned item, in order:

1. State the technique name and the target.
2. Apply the **smallest** version of the change.
3. Run tests (or the closest available check: build, typecheck, lint).
4. Report: `✅ <technique> at <location> — tests green`.
5. Stop immediately and revert the step if tests break.

### A4. Done report

- Table of applied techniques (technique / location / why)
- Behavior unchanged: how it was verified
- Leftover smells intentionally skipped, one line each (Rule of Three, near-ship risk, rewrite candidate…)

---

## PATH B — Execute a named technique

1. Confirm the technique fits (check motivation in `references/techniques.md`; if a better-fitting one exists, say so in one line, then respect the user's choice).
2. Follow the mechanics as baby steps (A3 loop).
3. If the technique has a known inverse (Extract ↔ Inline, Hide Delegate ↔ Remove Middle Man), sanity-check direction before starting.

## PATH C — Judgment call (refactor or not?)

Answer with a **decision + one reason**, using:

| Situation | Call |
|-----------|------|
| About to add a feature and structure fights you | **Preparatory refactoring** — refactor first, then add |
| Can't understand the code | **Comprehension refactoring** — refactor until it explains itself |
| Third time duplicating something | **Rule of Three** — now refactor |
| Passing by minor mess | **Litter-pickup** — small opportunistic cleanup only |
| Code should be rewritten; ship is near; no tests + high risk | **Don't refactor** |

---

## Anti-patterns (refuse these)

- Big-bang rewrite labeled "refactoring"
- Refactoring + behavior change in the same step (wearing both hats)
- Renaming/moving things with no smell-based justification
- "Improving" performance during refactoring without measurement
- Adding speculative abstractions (interfaces, layers) for imaginary futures
- Claiming behavior preserved without any test/build/check evidence

## Done criteria

You are done when the user has: a diagnosis table, a sequence of named baby-step
changes each verified green, and a one-line list of what was deliberately left alone —
not a book chapter summary.
