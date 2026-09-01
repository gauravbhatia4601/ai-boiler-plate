# AI boilerplate

Copy this directory into a new project and it's AI-ready: instructions, permissions, and skills preconfigured for Claude Code (and anything else that reads AGENTS.md).

## Use it

```sh
cp -R ai-boiler-plate/. your-project/   # or cp -R . new-project/ from a copy of this repo
cd your-project
```

Open Claude Code in the new project and it will prompt you to run `/setup`: it interviews you (project, stack, commands, style, boundaries) and fills every `TODO` in `AGENTS.md` for you. Until that's done, the AI is **hard-blocked** from doing any project work — it can only run `/setup` and edit `AGENTS.md`. After `/setup`, you're free to build whatever the project is.

## What's inside

| Path | Purpose |
|---|---|
| `AGENTS.md` | Source of truth for all agents: project, stack, commands, style, boundaries. |
| `CLAUDE.md` | One line that imports `AGENTS.md` into Claude Code. |
| `.claude/settings.json` | Read-only permission allowlist, hooks, and enabled plugins. |
| `.claude/hooks/block-env-guard` | PreToolUse hook: blocks committing or writing `.env` files. |
| `.claude/hooks/setup-gate` | Until AGENTS.md is configured: every prompt nags `/setup`, and all mutating tools are hard-blocked (only AGENTS.md and `.claude/**` edits + read-only commands pass). |
| `.claude/skills/setup/` | `/setup` skill: interviews you and rewrites AGENTS.md for your project. |
| `.claude/skills/verify/` | `/verify` skill: run build+test+lint before claiming done. |

Plugins shipped enabled: `ponytail`, `mattpocock-skills`, `code-review`. Teammates get them on first open (they still need the marketplaces added once per machine via `/plugin`).

## Verify it works

```sh
echo '{"tool_name":"Bash","tool_input":{"command":"git add .env"}}' | python3 .claude/hooks/block-env-guard; echo "exit=$?"   # expect exit=2
echo '{"tool_name":"Write","tool_input":{"file_path":"/x/.env"}}' | python3 .claude/hooks/block-env-guard; echo "exit=$?"    # expect exit=2
echo '{"tool_name":"Bash","tool_input":{"command":"git add README.md"}}' | python3 .claude/hooks/block-env-guard; echo "exit=$?"  # expect exit=0
```

Setup gate (fire = blocked/JSON, silent = configured):

```sh
H=<path-to-boilerplate>/.claude/hooks/setup-gate
cd /tmp && rm -rf gate-test && mkdir -p gate-test && cd gate-test
printf '# AGENTS.md\nA language-agnostic boilerplate that makes a new project AI-ready.\n' > AGENTS.md
echo '{"hook_event_name":"PreToolUse","tool_name":"Bash","tool_input":{"command":"npm test"}}' | CLAUDE_PROJECT_DIR=/tmp/gate-test python3 $H; echo "exit=$?"   # expect exit=2 (hard block)
echo '{"hook_event_name":"PreToolUse","tool_name":"Bash","tool_input":{"command":"grep -c TODO AGENTS.md"}}' | CLAUDE_PROJECT_DIR=/tmp/gate-test python3 $H; echo "exit=$?"  # expect exit=0 (read-only passes)
echo '{"prompt":"hi"}' | CLAUDE_PROJECT_DIR=/tmp/gate-test python3 $H   # expect SETUP REQUIRED JSON
printf '## Project\nMy app.\n' > AGENTS.md
echo '{"hook_event_name":"PreToolUse","tool_name":"Bash","tool_input":{"command":"npm test"}}' | CLAUDE_PROJECT_DIR=/tmp/gate-test python3 $H; echo "exit=$?"   # expect exit=0 (unlocked)
```

## Works with

| Harness | What it gets |
|---|---|
| Claude Code | Everything: AGENTS.md rules, permissions, hooks, skills, plugins. |
| Codex, Cursor, Windsurf, Zed, opencode, pi, Goose, Crush, Jules, Amp | Reads `AGENTS.md` natively — rules only. |
| Gemini CLI | `GEMINI.md` → symlink to `AGENTS.md`. |
| GitHub Copilot | `.github/copilot-instructions.md` → symlink to `AGENTS.md`. |
| Cline | `.clinerules` → symlink to `AGENTS.md`. |
| Roo Code | `.roorules` → symlink to `AGENTS.md`. |
| Aider | No auto-read; start with `aider --read AGENTS.md` (`CONVENTIONS.md` symlink provided for the `--read CONVENTIONS.md` habit). |

opencode additionally reuses Claude Code skills from `.claude/skills/` — `/verify` and `/setup` work there too.

Enforcement (hooks) is Claude Code-only. Other harnesses follow `AGENTS.md` as instructions — the rules still apply, just without a wall in front of them.

## Notes

- **MCP servers**: add a `.mcp.json` in the project root when a project needs one; don't pre-add an empty one.
- **Skills**: add project-specific ones under `.claude/skills/<name>/SKILL.md` as they earn their place.