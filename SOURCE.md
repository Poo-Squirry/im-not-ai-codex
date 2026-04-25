# Source Sync

This Codex plugin is a community port of:

- Original repo: https://github.com/epoko77-ai/im-not-ai
- Original version label: `v1.3.1`
- Original base commit used for this port: `2fc2e3b62dcee40af4d9b7d239ae0a7a463b2cee`
- Codex plugin version: `v1.3.1`
- Sync date: `2026-04-26`

## File Mapping

- `.claude/skills/humanize-korean/references/*.md` -> `skills/humanize-korean/references/*.md`
- `.claude/agents/*.md` -> `skills/humanize-korean/references/agents/*.md`
- `assets/social-preview.png` -> `assets/social-preview.png`
- `LICENSE` -> `LICENSE`

## Codex Adaptation Notes

- Claude Code slash commands are not copied as runtime commands. Codex uses the `humanize-korean` skill trigger instead.
- Claude Code `Agent`, `TeamCreate`, and `model: opus` calls are adapted into Codex role passes inside one skill workflow.
- Bundled reference files are treated as read-only. New pattern candidates should be written under `_workspace/{run_id}/06_pattern_candidates.md` instead of mutating installed plugin files.
- The original README remains the source of truth for the Claude Code edition. This repository is only the Codex plugin port.

## Version Policy

This Codex plugin intentionally follows the original `epoko77-ai/im-not-ai` version number. A release named `v1.3.1` means this repository contains the Codex plugin port for original `im-not-ai v1.3.1`.
