---
name: verify
description: Run the project's build, tests, and lint from AGENTS.md before claiming any task is done. Use when finishing a change, before a commit, or when asked to check the project.
---

# Verify

1. Read `AGENTS.md` → **Commands** section.
2. Run lint/format, then build, then tests — in that order, stop at the first failure.
3. Report pass/fail per command with the failing output verbatim. Do not summarize "mostly passing".
4. If a command is still `TODO` in AGENTS.md, say so — do not invent one.