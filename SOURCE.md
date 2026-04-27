# Source Sync

This Codex plugin is a community port of:

- Original repo: https://github.com/epoko77-ai/im-not-ai
- Original version label: `v1.5.0`
- Original release tag: `v1.5.0`
- Original release tag commit: `66f8399cff286d9758bf86c2ba3aa222ecb1a4f4`
- Upstream docs commit checked during this sync: `3fab1d8451170f10270b3168a97dd05fca010f6f`
- Codex plugin version: `v1.5.0`
- Sync date: `2026-04-27`

`v1.5.0` runtime files are identical between the release tag and upstream `main`; after the tag, upstream changed `README.md` only. This port therefore syncs the v1.5 runtime files and also reflects the current upstream README notes.

## File Mapping

- `.claude/skills/humanize-korean/references/*.md` -> `skills/humanize-korean/references/*.md`
- `.claude/agents/*.md` -> `skills/humanize-korean/references/agents/*.md`
- `assets/social-preview.png` -> `assets/social-preview.png`
- `LICENSE` -> `LICENSE`

## v1.5 Sync Notes

Upstream v1.5 removed the v1.2-v1.4 hot-path additions:

- `author-context-schema.md`
- `pattern-candidates.md`
- `promotion-checklist.md`
- `sample-collection.md`

This Codex port removes those files as well.

Upstream v1.5 added:

- `references/quick-rules.md`
- `agents/humanize-monolith.md`

This Codex port preserves both files and adapts the workflow so Codex runs the monolith behavior inside the `humanize-korean` skill instead of calling Claude Code's `Agent` tool.

## Codex Adaptation Notes

- Claude Code slash commands are not copied as runtime commands. Codex uses the `humanize-korean` skill trigger instead.
- Claude Code `Agent`, `TeamCreate`, and `model: opus` calls are adapted into Codex role passes inside one skill workflow.
- Fast mode is the default and uses `quick-rules.md` plus the `humanize-monolith` behavior reference.
- Strict mode preserves the 5-role pipeline behavior using the full taxonomy and playbook.
- Bundled reference files are treated as read-only. If a run discovers a suspicious new pattern, write it under `_workspace/{run_id}/06_pattern_candidates.md` instead of mutating installed plugin files.
- The original README remains the source of truth for the Claude Code edition. This repository is only the Codex plugin port.

## Version Policy

This Codex plugin intentionally follows the original `epoko77-ai/im-not-ai` version number. A release named `v1.5.0` means this repository contains the Codex plugin port for original `im-not-ai v1.5.0`.
