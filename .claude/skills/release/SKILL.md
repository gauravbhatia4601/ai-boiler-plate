---
name: release
description: Cut a release — version bump and changelog from commits since the last tag. Use when the user asks to release, cut a version, or update the changelog.
---

# Release

1. Read `AGENTS.md` → Commands for the project's version/release conventions. If none are declared, ask — don't invent a scheme.
2. `git log <last-tag>..HEAD` and group commits: Added / Changed / Fixed. One line each, user-facing language.
3. Bump the version everywhere the project declares it, append the changelog entry, and commit as `Release vX.Y.Z`.
4. Never publish, tag-and-push, or deploy unless asked explicitly.