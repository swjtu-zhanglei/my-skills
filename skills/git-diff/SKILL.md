---
name: git-diff
description: "Show every code change as a line-by-line git diff. MUST use when you have edited code and are reporting back, when the user asks what changed or to see the diff, and when reviewing a branch/commit/PR. Output is real `git diff` output — never full-file before/after dumps or prose summaries."
license: MIT
---

# Git Diff

Show the change itself, line by line. Always respond in the user's language.

## Hard rules

1. Real `git diff` output only — never hand-write, paraphrase, or reorder hunks.
2. Never paste whole before/after files as the change view.
3. No diff, no claim: every change you report has its hunk in the same message.
4. Read the diff before reviewing it.
5. Never commit, amend, push, or reset unless asked.

## Get the diff

`git --no-pager diff --no-color` (no pager, no ANSI).

| Situation | Command |
|---|---|
| Uncommitted edits | `git diff -- <path>` |
| Staged | `git diff --staged -- <path>` |
| One commit | `git show <sha> -- <path>` |
| Branch / PR | `git diff <base>...<head> -- <path>` |
| Untracked / no repo | `git diff --no-index -- <old> <new>` (exit 1 = differs) |
| Not applied yet | edit a temp copy → `git diff --no-index` → label `proposed` |

## Output

1. **Stat** — one line of intent + `git diff --stat` block.
2. **Diff** — one fenced `diff` block per file, git order; keep real
   `diff --git a/… b/…`, `---`/`+++`, `@@`, 3 context lines, and the real
   `new file` / `deleted` / `rename` / `Binary files differ` lines. Never invent
   content or line numbers.
3. **Review points** — `path:line` + what to check + `P0`/`P1`/`P2`, risk order.
4. **Verification** — tests/build/lint actually run; otherwise say "not run".
5. **Cap**: 200 diff lines / 5 files; beyond that show `--stat` and only the
   review-critical hunks, eliding the rest as `… (N lines omitted)`.

## Reviewing someone else's change

Fetch the real diff (`git show`, `git diff base...head`, `gh pr diff`) before the
first comment; state the range reviewed; give fixes as `diff` blocks; "no issues"
only after reading it.

## Never

- full-file before/after dumps · "I changed X" with no hunk · prose-only summaries
- reviewing a diff you did not fetch · 2000-line dumps with no stat or priorities
- `checkout` / `reset` / `clean` to tidy what you show · committing or pushing alone

## Done

The reviewer can judge the change from the message alone — real diff, ordered
review points, verification evidence — without opening a file.
