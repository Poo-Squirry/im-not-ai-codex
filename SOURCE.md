# Source Sync

This Codex plugin is a community port of:

- Original repo: https://github.com/epoko77-ai/im-not-ai
- Original version label: `v2.0.0`
- Original release tag: `v2.0.0`
- Original upstream commit used for this sync: `807172694d75a9e9e0390f93331387c389ce748e`
- Codex plugin version: `v2.0.0`
- Sync date: `2026-05-08`

Upstream `v2.0.0` keeps the v1.6.1 monolith/5-role runtime contract intact while expanding the reference layer: Korean translation-scholarship patterns, post-editese metrics, new v2 baselines, and a refreshed social preview.

## File Mapping

- `.claude/skills/humanize-korean/references/*.md` -> `plugins/im-not-ai/skills/humanize-korean/references/*.md`
- `.claude/skills/humanize-korean/references/*.json` -> `plugins/im-not-ai/skills/humanize-korean/references/*.json`
- `.claude/skills/humanize-korean/references/*.py` -> `plugins/im-not-ai/skills/humanize-korean/references/*.py`
- `.claude/agents/*.md` -> `plugins/im-not-ai/skills/humanize-korean/references/agents/*.md`
- `scripts/prepare_monolith_input.py` -> `plugins/im-not-ai/scripts/prepare_monolith_input.py` with Codex plugin paths adapted and `metrics_v2.py` preferred when present
- `tests/test_metrics.py` -> `plugins/im-not-ai/tests/test_metrics.py` with Codex plugin paths adapted
- `tests/test_metrics_v2.py` -> `plugins/im-not-ai/tests/test_metrics_v2.py` with Codex plugin paths adapted
- `assets/social-preview.png` -> `plugins/im-not-ai/assets/social-preview.png`
- `assets/social-preview-v1.png` -> `plugins/im-not-ai/assets/social-preview-v1.png`
- `scripts/make_thumbnail.py` -> `plugins/im-not-ai/scripts/make_thumbnail.py` with the footer URL and font fallback adapted for this Codex port
- `scripts/build_social_preview_v2.py` -> `plugins/im-not-ai/scripts/build_social_preview_v2.py` with the footer URL and font fallback adapted for this Codex port
- `LICENSE` -> `LICENSE`

## v2.0.0 Sync Notes

Upstream changed these files after the previous `6138697` sync:

- `ai-tell-taxonomy.md`
- `quick-rules.md`
- `rewriting-playbook.md`
- `baseline_v2.json`
- `metrics_v2.py`
- `scholarship.md`
- new supporting role prompts: `korean-translation-scholar`, `post-editese-metric-engineer`, `quick-rules-integrator`, `taxonomy-gap-analyzer`, `translationese-research-distiller`
- `scripts/prepare_monolith_input.py`
- `tests/test_metrics_v2.py`
- `README.md`
- `assets/social-preview.png`
- `assets/social-preview-v1.png`
- `scripts/make_thumbnail.py`
- `scripts/build_social_preview_v2.py`

Codex-specific adaptations:

- Claude Code slash commands are not copied as runtime commands. Codex uses the `humanize-korean` skill trigger instead.
- Claude Code `Agent`, `TeamCreate`, and `model: opus` calls are adapted into Codex role passes inside one skill workflow.
- Fast mode computes v2 metrics when local script execution is available, falls back to v1.6 metrics if needed, then reads `01_input_with_metrics.txt`.
- Fast mode follows upstream `v1.6.1` and writes `final.md` only; strict mode still writes `final.md` plus `summary.md`.
- GitHub owner links and install commands remain `Squirbie/im-not-ai-codex`.
- Bundled reference files are treated as read-only. If a run discovers a suspicious new pattern, write it under `_workspace/{run_id}/06_pattern_candidates.md` instead of mutating installed plugin files.

## Prior Sync Notes

- `2026-05-07`: marketplace source path was changed from root `./` to `./plugins/im-not-ai` so Codex CLI 0.128.x can discover the plugin from `/plugins` on Windows and other strict path validators.
- `2026-05-02`: upstream `ebe1328` run_id marker-file workflow fix was adapted into `plugins/im-not-ai/skills/humanize-korean/SKILL.md`.
- `2026-04-29`: upstream README community-port links from PR #12 and PR #15 were reflected while preserving Squirbie owner links.
- `v1.5.0`: the Codex port adopted the monolith fast path, `quick-rules.md`, and `humanize-monolith.md`, and removed the v1.2-v1.4 hot-path reference files that upstream removed.

## Version Policy

This Codex plugin intentionally follows the original `epoko77-ai/im-not-ai` version number. A release named `v2.0.0` means this repository contains the Codex plugin port for original `im-not-ai v2.0.0`.
