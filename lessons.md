# Lessons

Problems that cost time and the fixes that worked. Unlike decisions, lessons
are never archived or pruned: the answer you need months later is exactly the
one nobody remembers. Add a new entry at the top. Edit an old one only to
correct it or to note a case where it didn't hold.

Search here before solving a problem that feels familiar:
`./scripts/search.sh <one distinctive word>`

<!--
Entry template:

## <Short title naming the symptom>
- **Problem:** <what went wrong, and how it showed up>
- **Fix:** <what worked>
- **Doesn't apply when:** <the case where following this would be a mistake>
-->

## 2026-10-23: Force-add binaries and push in the same turn
- **Mistake:** Reference PDFs and DOCX edits lived only in the sandbox; user could not see them on GitHub because `*.pdf` is gitignored and nothing was force-added/pushed.
- **Fix:** Always `git add -f` for thesis binaries, commit, then push with the one-shot credential helper before telling the user the work is in the repo.
- **Check:** `git status` must show "up to date with origin/main" (not "ahead by N commits").
