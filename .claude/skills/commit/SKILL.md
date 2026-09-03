---
name: commit
description: Create a clean one-concern-per-commit from the current diff. Use when the user asks to commit, or to wrap up a finished change.
---

# Commit

1. Read `AGENTS.md` → Git rules. Run `git status` and `git diff` (staged and unstaged).
2. If the diff spans multiple concerns, propose splitting into several commits and confirm before proceeding.
3. Stage only the files belonging to the concern — never `git add -A` blindly. `.env` files stay local (the hook blocks them anyway).
4. If the change is non-trivial, run `/verify` first; report failures instead of committing red code.
5. Message: imperative subject ≤50 chars, no trailing period, matching AGENTS.md style. Body only for the "why", not the "what".
6. Commit. Never push unless asked.