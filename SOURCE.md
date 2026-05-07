# Source Sync

This Codex plugin is a community port of:

- Original repo: https://github.com/epoko77-ai/im-not-ai
- Original version label: `v1.6.1`
- Original release tag: `v1.6.1`
- Original upstream commit used for this sync: `6138697bc372c7352d8d91fcf22992ddd82fa02e`
- Codex plugin version: `v1.6.1`
- Sync date: `2026-05-07`

Upstream `v1.6.0` added the KatFish/LREAD quantitative metrics layer, new taxonomy patterns, `metrics.py`, `baseline.json`, `prepare_monolith_input.py`, and metrics tests.

Upstream `v1.6.1` then hotfixed fast-mode output: `humanize-monolith` now writes one `final.md` file containing the rewritten body plus a trailing `<!-- HUMANIZE-SUMMARY -->` metadata block, avoiding a second `summary.md` write in the fast path.

## File Mapping

- `.claude/skills/humanize-korean/references/*.md` -> `skills/humanize-korean/references/*.md`
- `.claude/skills/humanize-korean/references/*.json` -> `skills/humanize-korean/references/*.json`
- `.claude/skills/humanize-korean/references/*.py` -> `skills/humanize-korean/references/*.py`
- `.claude/agents/*.md` -> `skills/humanize-korean/references/agents/*.md`
- `scripts/prepare_monolith_input.py` -> `scripts/prepare_monolith_input.py` with Codex plugin paths adapted
- `tests/test_metrics.py` -> `tests/test_metrics.py` with Codex plugin paths adapted
- `assets/social-preview.png` -> `assets/social-preview.png`
- `LICENSE` -> `LICENSE`

## v1.6.1 Sync Notes

Upstream changed these runtime files after the previous `ebe1328` sync:

- `ai-tell-taxonomy.md`
- `quick-rules.md`
- `baseline.json`
- `metrics.py`
- `humanize-monolith.md`
- `scripts/prepare_monolith_input.py`
- `tests/test_metrics.py`
- `README.md`
- `.gitignore`

Codex-specific adaptations:

- Claude Code slash commands are not copied as runtime commands. Codex uses the `humanize-korean` skill trigger instead.
- Claude Code `Agent`, `TeamCreate`, and `model: opus` calls are adapted into Codex role passes inside one skill workflow.
- Fast mode computes metrics when local script execution is available, then reads `01_input_with_metrics.txt`.
- Fast mode follows upstream `v1.6.1` and writes `final.md` only; strict mode still writes `final.md` plus `summary.md`.
- GitHub owner links and install commands remain `Squirbie/im-not-ai-codex`.
- Bundled reference files are treated as read-only. If a run discovers a suspicious new pattern, write it under `_workspace/{run_id}/06_pattern_candidates.md` instead of mutating installed plugin files.

## Prior Sync Notes

- `2026-05-02`: upstream `ebe1328` run_id marker-file workflow fix was adapted into `skills/humanize-korean/SKILL.md`.
- `2026-04-29`: upstream README community-port links from PR #12 and PR #15 were reflected while preserving Squirbie owner links.
- `v1.5.0`: the Codex port adopted the monolith fast path, `quick-rules.md`, and `humanize-monolith.md`, and removed the v1.2-v1.4 hot-path reference files that upstream removed.

## Version Policy

This Codex plugin intentionally follows the original `epoko77-ai/im-not-ai` version number. A release named `v1.6.1` means this repository contains the Codex plugin port for original `im-not-ai v1.6.1`.
