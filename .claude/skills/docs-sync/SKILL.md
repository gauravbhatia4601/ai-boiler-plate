---
name: docs-sync
description: Check README and docs against the actual code and fix drift. Use when docs may be stale, after renames or command changes, or when asked to check documentation.
---

# Docs sync

1. Collect every checkable claim from `README.md` and other docs: commands, file paths, config keys, setup steps.
2. Verify each against the repo — does the command exist, does the path exist, does the config key match reality? Run commands read-only where safe.
3. Fix drift in place, in the doc's own voice. Don't rewrite style, only facts.
4. Claims you cannot verify (external services, credentials, CI internals): report them — never guess.