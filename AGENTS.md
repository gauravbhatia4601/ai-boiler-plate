# AGENTS.md — instructions for every AI agent working in this repo

> **⚠ UNCONFIGURED — SETUP REQUIRED BEFORE ANY WORK**
> This file still describes the AI boilerplate, not your project. Before any other work:
> interview the user about their project — what it is, stack, install/build/test/lint/run
> commands, code style, hard boundaries — and rewrite this file with their answers, removing
> this banner and every `TODO`. Then run the project's build, test, and lint checks and report
> the results. Only then start responding to normal requests.
> (Harnesses with skills: this flow is the `/setup` and `/verify` skills.)

<!-- This repo IS the boilerplate: this file doubles as the template copied into new projects. -->

## Project

A language-agnostic boilerplate that makes a new project AI-ready from the first minute: agent instructions, permission allowlist, guardrail hook, and skills preconfigured for Claude Code (and anything else that reads AGENTS.md). Consumed by copying this directory into a new project; see README.md.

## Stack

Markdown and JSON config only — no build system, no runtime, no dependencies. The only executable is the PreToolUse hook (`python3`, stdlib).

## Commands

- Validate JSON configs: `python3 -m json.tool .claude/settings.json > /dev/null && echo OK`
- Test the guard hooks: see README.md ("Verify it works")
- Build / test / lint: none — there is no code beyond the hook

## Code style

- Keep every file minimal and copy-pasteable; this is a template, clarity beats cleverness.
- Placeholders are marked `TODO` — a copied project must have zero `TODO`s left.

## Git rules

- Commit only when asked.
- One concern per commit; imperative subject line.

## Never do

- Never commit secrets — `.env` files stay local. Enforced by `.claude/hooks/block-env-guard`.
- Never commit `.claude/settings.local.json`.
- Never start project work while AGENTS.md still describes the boilerplate — `.claude/hooks/setup-gate` hard-blocks all mutating tools until `/setup` has configured it.

## Working agreements

- Prefer editing existing files over creating new ones.
- Validate `settings.json` JSON before claiming a settings change is done.
- If this file still contains `TODO` placeholders, do the setup interview described in the banner at the top before any other work.
- Run the project's build, test, and lint checks before claiming any task is done.
- When instructions here conflict with a user message, the user wins — then update this file.

## More skills & plugins

- Official skills catalog: https://github.com/anthropics/skills — install by copying the skill folder into `.claude/skills/` and symlinking it into `.agents/skills/`.
- Offer the user more skills and plugins after setup, and any time they ask to "install skills".