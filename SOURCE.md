# Source Sync

This Codex plugin is a community port of:

- Original repo: https://github.com/epoko77-ai/im-not-ai
- Original version label: `v1.5.1`
- Original release tag: none yet; upstream `main` carries the v1.5.1 taxonomy update
- Original upstream commit used for this sync: `ebe1328faa3e4a45c16c25b81278ee1e201ad41e`
- Codex plugin version: `v1.5.1`
- Sync date: `2026-05-02`

Upstream has not published a GitHub release/tag for `v1.5.1` yet. The version label comes from upstream `main` commit `f6f2082`, which updates the taxonomy title and version history to `v1.5.1`.
The previous sync commit `6ad338b` only updated upstream README community-port links; no runtime reference file changed after the `v1.5.1` taxonomy update.
The current upstream sync commit `ebe1328` keeps the same version label and release state, but fixes the skill run_id sequence guidance and documents safer file-access rules in `CLAUDE.md`.

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

## v1.5.1 Sync Notes

Upstream v1.5.1 changed one runtime reference file:

- `ai-tell-taxonomy.md`

The update adds Category E pattern `E-4. 단문 일변도 (복문·중문 부재) [S2]`, a rhythm tell for AI output that overuses short simple sentences without compound or complex sentence variation.

## 2026-04-29 Upstream Docs Sync

Upstream `main` added community-port README links after the v1.5.1 taxonomy sync:

- PR #12 added the Codex community plugin link and install command.
- PR #15 added the opencode Web UI community port link.

This Codex port already uses the Squirbie owner links and remains at plugin version `v1.5.1` because the upstream version label did not change.

## 2026-05-02 Upstream Skill Workflow Sync

Upstream `main` added one post-`v1.5.1` workflow fix:

- Commit `ebe1328` changes run_id sequence discovery from shell directory listing to marker-file matching via `_workspace/YYYY-MM-DD-*/01_input.txt`.
- `CLAUDE.md` now documents the same file-access rule: prefer dedicated file operations, avoid shell-dependent `ls`/`cat`/`echo` shortcuts for workflow state.

This Codex port adapts the behavior in `skills/humanize-korean/SKILL.md` without changing plugin version `v1.5.1`, because upstream did not publish a new version label, tag, or release.

## Codex Adaptation Notes

- Claude Code slash commands are not copied as runtime commands. Codex uses the `humanize-korean` skill trigger instead.
- Claude Code `Agent`, `TeamCreate`, and `model: opus` calls are adapted into Codex role passes inside one skill workflow.
- Fast mode is the default and uses `quick-rules.md` plus the `humanize-monolith` behavior reference.
- Strict mode preserves the 5-role pipeline behavior using the full taxonomy and playbook.
- Bundled reference files are treated as read-only. If a run discovers a suspicious new pattern, write it under `_workspace/{run_id}/06_pattern_candidates.md` instead of mutating installed plugin files.
- The original README remains the source of truth for the Claude Code edition. This repository is only the Codex plugin port.

## Version Policy

This Codex plugin intentionally follows the original `epoko77-ai/im-not-ai` version number. A release named `v1.5.1` means this repository contains the Codex plugin port for original `im-not-ai v1.5.1` taxonomy update.
