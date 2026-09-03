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
2. Rewrite `AGENTS.md` fully from the answers. Keep the section structure (Project / Stack / Commands / Code style / Git rules / Never do / Working agreements) and the boilerplate-invariant rules (Git rules, ".env stays local", working agreements, "More skills & plugins"). Remove the boilerplate description, the UNCONFIGURED banner at the top, and every `TODO`.
3. Verify: `grep -c TODO AGENTS.md` → 0, and the boilerplate description is gone.
4. Plugins (Claude Code only — skip elsewhere): check whether `ponytail@ponytail`,
   `mattpocock-skills@claude-plugins-official`, `code-review@claude-plugins-official` are
   installed (`/plugin` list). For any missing one, run:
   `claude plugin install ponytail@ponytail` (and likewise for the others).
5. If any plugin or skill was installed during this session, tell the user: "restart the
   harness (or reload plugins) so they activate — then come back here."
6. Offer extras — ask the user: "Want more skills or plugins for this kind of work?"
   - If yes, skills: fetch https://github.com/anthropics/skills to list the official catalog,
     shortlist the ones relevant to this project's stack and work, present them, and install
     the user's picks: copy each skill folder into `.claude/skills/`, then
     `ln -sfn "../../.claude/skills/<name>" ".agents/skills/<name>"` for the other harnesses.
   - If yes, plugins (Claude Code only): fetch the `claude-plugins-official` marketplace list,
     shortlist relevant ones, present them, and run
     `claude plugin install <name>@claude-plugins-official` for the user's picks.
   - If anything new was installed this session, remind the user to restart/reload first.
7. Tell the user: "AGENTS.md configured — you're ready to start." Until then, no other work.