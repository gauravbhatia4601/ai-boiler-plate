---
name: setup
description: Interview the developer and rewrite AGENTS.md for this project. Compulsory on first run in a project cloned from the AI boilerplate — use when AGENTS.md still describes the boilerplate or contains TODOs.
---

# Setup

This project was cloned from the AI boilerplate. Its `AGENTS.md` still describes the *boilerplate*, not this project. Interview the user, then rewrite the file.

1. Ask, in one round (AskUserQuestion for enumerable choices, free text otherwise):
   - What is the project? One paragraph: what it is, who it's for.
   - Stack: languages, frameworks, package manager, runtime versions.
   - Commands: install / build / test / lint / run locally.
   - Code style rules — short, no essays.
   - Extra "never do" boundaries beyond ".env stays local".
2. Rewrite `AGENTS.md` fully from the answers. Keep the section structure (Project / Stack / Commands / Code style / Git rules / Never do / Working agreements) and the boilerplate-invariant rules (Git rules, ".env stays local", working agreements). Remove the boilerplate description and every `TODO`.
3. Verify: `grep -c TODO AGENTS.md` → 0, and the boilerplate description is gone.
4. Tell the user: "AGENTS.md configured — you're ready to start." Until then, no other work.