# AGENTS.md — instructions for every AI agent working in this repo

> **⚠ UNCONFIGURED — SETUP REQUIRED BEFORE ANY WORK**
> This file still describes the AI boilerplate, not your project. Run `/setup` now: it interviews
> the user and rewrites this file. When done, run `/verify`. Do not start any other work until
> both have run. (This banner disappears once `/setup` rewrites the file.)

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
- If this file still contains `TODO` placeholders, run `/setup` before any other work, then run `/verify` — on every harness, not just Claude Code.
- Run `/verify` before claiming any task is done.
- When instructions here conflict with a user message, the user wins — then update this file.