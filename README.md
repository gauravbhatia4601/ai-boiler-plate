# AI Boilerplate

A folder you copy into every new project so your AI coding tool is set up from minute one — instructions, skills, and guardrails already in place. Works with every major harness: Claude Code, Codex, Cursor, Gemini CLI, pi, Windsurf, Cline, Roo Code, Copilot, opencode, Aider, Zed, Goose, and more.

---

## Start here (3 steps)

**Step 1 — Copy the folder into your new project**

```sh
cp -R ai-boiler-plate/. your-project/
cd your-project
```

**Step 2 — Open your AI tool and let it set itself up**

Open any harness (Claude Code, Cursor, Codex, pi, …) in the project and type anything — even just "hi".

The top of `AGENTS.md` has a banner that tells the AI: "this project is not set up yet — interview the user first." So the AI will ask you a few questions (what the project is, which stack, which commands) and write them into `AGENTS.md`.

**Step 3 — Start building**

Once `AGENTS.md` has no `TODO`s left, the banner is gone and the AI starts normal work. From then on it follows your project's rules and runs the project's build/test/lint checks before claiming anything is done.

---

## Want more skills or plugins?

The official skills catalog lives at **https://github.com/anthropics/skills**. You don't need to visit it — the flow installs for you:

1. When `/setup` finishes, it asks: *"Want more skills or plugins for this kind of work?"*
2. If you say yes, the AI fetches the catalog, picks out the ones relevant to your project's stack, and shows you the list.
3. You choose; it installs the picks — skills into `.claude/skills/` (+ symlinked into `.agents/skills/` so every harness sees them), plugins via `claude plugin install` (Claude Code only).
4. If anything was installed mid-session, it tells you to restart the harness so it activates.

You can do the same any time later by asking: "install more skills" — the pointer lives in `AGENTS.md` → *More skills & plugins*.

---

## The 5 skills that come with it

| Skill | What it does |
|---|---|
| `/setup` | Interviews you, rewrites `AGENTS.md` for your project. Run first, once. |
| `/verify` | Runs build + test + lint and reports pass/fail. Run before saying "done". |
| `/commit` | Writes a clean, one-concern-per-commit message from your diff. |
| `/docs-sync` | Checks that README/docs match the actual code, fixes drift. |
| `/release` | Bumps the version and writes changelog entries from recent commits. |

Skills work as slash commands in Claude Code, pi, and opencode. In other harnesses the same behavior comes from the `AGENTS.md` rules — just ask for it in plain words ("verify", "commit this", "cut a release").

---

## Inside the folder

**The instructions (read by every harness):**

| File | What it is |
|---|---|
| `AGENTS.md` | The rulebook. Every AI tool reads it automatically. Has a setup banner + `TODO` slots you fill once per project. |
| `CLAUDE.md`, `GEMINI.md`, `CONVENTIONS.md`, `.github/copilot-instructions.md` | One-line pointer files (or symlinks) so each tool's expected filename loads the same `AGENTS.md`. Write rules once. |
| `.clinerules`, `.roorules` | Same idea for Cline and Roo Code. |

**The skills (behaviors you can invoke):**

| Folder | What it is |
|---|---|
| `.claude/skills/<name>/SKILL.md` | The actual skill files. Source of truth. |
| `.agents/skills/<name>` | Symlinks to the same skills — this is the neutral path Codex, pi, Gemini CLI, Cursor, Roo, Cline, and Windsurf read. |

**The guardrails (Claude Code only — no other tool has hooks):**

| File | What it is |
|---|---|
| `.claude/hooks/setup-gate` | Until setup is done, every prompt reminds the AI to set up first, and it cannot edit files or run commands (except the setup itself). |
| `.claude/hooks/block-env-guard` | Always on: the AI can never commit or write `.env` files — secrets stay local. |
| `.claude/settings.json` | Permissions (safe read-only commands pre-approved), the two hooks above, and plugins switched on. |

**The MCP servers (AI-connected tools):**

| File | Which tool reads it |
|---|---|
| `.mcp.json` | Claude Code |
| `.cursor/mcp.json` | Cursor |
| `.vscode/mcp.json` | VS Code / Copilot |
| `.gemini/settings.json` | Gemini CLI |
| `.codex/config.toml` | Codex |
| `.roo/mcp.json` | Roo Code |

All six point at one server: `codebase-memory-mcp` (indexes your codebase so the AI can search it structurally).

**Extras:**

| File | What it is |
|---|---|
| `README.md` | This file — delete it after copying if you don't want it in your project. |
| `LICENSE` | MIT — anyone may use this boilerplate. |

---

## One-time installs (per machine, not per project)

**The MCP server binary** (all tools need this on your PATH):

```sh
curl -fsSL https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.sh | bash
```

**Plugins (Claude Code only):** already pre-enabled in the config — `/setup` installs any that are missing and tells you to restart. Manual install:

```sh
claude plugin install ponytail@ponytail
claude plugin install mattpocock-skills@claude-plugins-official
claude plugin install code-review@claude-plugins-official
```

Notes: Windsurf and Cline configure MCP per user (no project file exists), and **pi has no MCP support by design**. Other harnesses have no plugins. Where something can't install, the `AGENTS.md` rules still apply as instructions.

---

## Check it works

```sh
# .env guard: blocked (exit 2), then allowed (exit 0)
echo '{"tool_name":"Bash","tool_input":{"command":"git add .env"}}' | python3 .claude/hooks/block-env-guard; echo $?
echo '{"tool_name":"Bash","tool_input":{"command":"git add README.md"}}' | python3 .claude/hooks/block-env-guard; echo $?

# setup gate: blocked (exit 2) while AGENTS.md is unconfigured
echo '{"hook_event_name":"PreToolUse","tool_name":"Bash","tool_input":{"command":"npm test"}}' | CLAUDE_PROJECT_DIR="$PWD" python3 .claude/hooks/setup-gate; echo $?
```

CI runs these same checks on every push to this repo.

## If the setup prompt doesn't appear

1. The `AGENTS.md` banner is the universal mechanism — it works in every harness on every prompt. If the AI ignores it, say: "follow the banner at the top of AGENTS.md".
2. The Claude Code hook loads only at session start: **restart the tool after copying the files**, and accept the folder-trust prompt.

## Adding a project-specific skill later

Create `.claude/skills/<name>/SKILL.md`, then expose it everywhere:

```sh
ln -sfn "../../.claude/skills/<name>" ".agents/skills/<name>"
```

## License

MIT — see [LICENSE](LICENSE).