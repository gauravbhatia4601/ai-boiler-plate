# AI boilerplate

Copy this directory into a new project and it's AI-ready: instructions, permissions, and skills preconfigured for Claude Code (and anything else that reads AGENTS.md).

## Use it

```sh
cp -R ai-boiler-plate/. your-project/   # or cp -R . new-project/ from a copy of this repo
cd your-project
```

Open any AI harness (Claude Code, Cursor, Codex, Gemini CLI, pi, opencode, …) in the new project. The banner at the top of `AGENTS.md` — which every harness auto-reads — orders it to interview you first (project, stack, commands, style, boundaries), rewrite the file, then run build/test/lint. No harness-specific machinery required; the flow works everywhere.

## Commands this project provides

Slash commands, available where the harness supports skills (Claude Code, opencode). Everywhere else, the `AGENTS.md` banner and rules drive the same behaviors — no commands needed.

| Command | What it does |
|---|---|
| `/setup` | Interviews you and rewrites `AGENTS.md` — compulsory on first run. |
| `/verify` | Runs build + test + lint before anything is claimed done. |
| `/commit` | Clean one-concern-per-commit from the diff. |
| `/docs-sync` | Checks docs against actual code, fixes drift. |
| `/release` | Version bump + changelog from commits since the last tag. |

## What's inside

| Path | Purpose |
|---|---|
| `AGENTS.md` | Source of truth for all agents: project, stack, commands, style, boundaries. |
| `CLAUDE.md` | One line that imports `AGENTS.md` into Claude Code. |
| `.claude/settings.json` | Permission allowlist, hooks, plugins (pre-enabled), marketplace config. |
| `.mcp.json` | Project MCP servers — ships `codebase-memory-mcp`. |
| `.claude/hooks/block-env-guard` | PreToolUse hook: blocks committing or writing `.env` files. |
| `.claude/hooks/setup-gate` | Until AGENTS.md is configured: every prompt nags `/setup`, and all mutating tools are hard-blocked (only AGENTS.md and `.claude/**` edits + read-only commands pass). |
| `.claude/skills/setup/` | `/setup` skill: interviews you and rewrites AGENTS.md for your project. |
| `.claude/skills/verify/` | `/verify` skill: run build+test+lint before claiming done. |
| `.claude/skills/commit/` | `/commit` skill: one-concern-per-commit messages from the diff. |
| `.claude/skills/docs-sync/` | `/docs-sync` skill: check docs against actual code, fix drift. |
| `.claude/skills/release/` | `/release` skill: version bump + changelog from commits since last tag. |

Plugins pre-enabled: `ponytail`, `mattpocock-skills`, `code-review` (Claude Code only — no other harness has plugins). The `/setup` flow installs any missing ones and tells you to restart the harness so they activate. Manual install, one command per machine:

```sh
claude plugin install ponytail@ponytail
claude plugin install mattpocock-skills@claude-plugins-official
claude plugin install code-review@claude-plugins-official
```

## MCP servers

`codebase-memory-mcp` ships in four project-scope configs, one per harness format:

| File | Harness |
|---|---|
| `.mcp.json` | Claude Code (prompts once per project to approve) |
| `.cursor/mcp.json` | Cursor |
| `.vscode/mcp.json` | VS Code / GitHub Copilot |
| `.gemini/settings.json` | Gemini CLI |

Codex, pi, and Windsurf configure MCP at user level only — no project file exists for them; register the server once per machine. Requires the binary on your PATH — install once per machine:

```sh
curl -fsSL https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.sh | bash
```

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
| Cursor (skills) | `.cursor/rules/*.mdc` → symlinks to each `.claude/skills/*/SKILL.md` — all five skills available as rules. |
| Aider | No auto-read; start with `aider --read AGENTS.md` (`CONVENTIONS.md` symlink provided for the `--read CONVENTIONS.md` habit). |

opencode additionally reuses Claude Code skills from `.claude/skills/` — `/verify` and `/setup` work there too.

Enforcement (hooks) is Claude Code-only. Other harnesses follow `AGENTS.md` as instructions — the rules still apply, just without a wall in front of them.

## Notes

- **Skills**: add project-specific ones under `.claude/skills/<name>/SKILL.md` as they earn their place.

## Troubleshooting: the setup nag didn't fire

The primary layer is harness-agnostic: the banner at the top of `AGENTS.md`, which every harness auto-reads on every prompt. If an agent ignores it, say "follow the banner at the top of AGENTS.md".

The optional second layer is the Claude Code hard-block hook — it exists only there, loads only at session start (**restart after copying files**), and needs the folder trusted. All other harnesses have no hook mechanism at all; the banner is their enforcement.

Test the Claude hook directly in the copied project:

```sh
echo '{"hook_event_name":"PreToolUse","tool_name":"Bash","tool_input":{"command":"npm test"}}' | CLAUDE_PROJECT_DIR="$PWD" python3 .claude/hooks/setup-gate; echo "exit=$?"  # expect exit=2
```