---
name: humanize-korean
description: Use when the user asks to make Korean AI-generated writing sound naturally human while preserving meaning. Trigger phrases include "AI 티 없애줘", "GPT 문체 제거", "사람이 쓴 것처럼 윤문", "번역투 제거", "한글 AI 윤문", "AI 글 사람처럼", and "humanize Korean". This Codex skill is a community plugin port of epoko77-ai/im-not-ai v1.3.1.
---

# Humanize Korean for Codex

Codex plugin port of `epoko77-ai/im-not-ai`. Detect Korean AI-writing tells, rewrite only the affected style spans, then audit that meaning is preserved.

Original source baseline:

- Original repo: `https://github.com/epoko77-ai/im-not-ai`
- Original version label: `v1.3.1`
- Original base commit: `2fc2e3b62dcee40af4d9b7d239ae0a7a463b2cee`

## First Response

When this skill starts, print this line before continuing:

```text
humanize-korean codex-port v1.3.1 / original im-not-ai v1.3.1 / voice profile {미주입|주입(<author-context.yaml 경로>)} / run_id: {YYYY-MM-DD-NNN}
```

Then continue the workflow in the same response or turn.

## Core Rules

1. Meaning is sacred. Facts, claims, dates, numbers, names, products, institutions, legal text, formulas, and direct quotes must stay intact.
2. Rewrite only style, rhythm, expression, and structure that are grounded in detected findings.
3. Preserve genre. Do not turn a report into an essay, a column into literature, or a formal speech into casual chat.
4. Do not over-polish. Warn over 30 percent change rate. Stop and report over 50 percent change rate.
5. Do not mutate bundled reference files. Write all run artifacts under the current working directory's `_workspace/{run_id}/`.
6. This Codex port does not require Claude Code `Agent`, `TeamCreate`, slash commands, or `model: opus`. Run the role passes in this skill workflow. If the user explicitly asks for parallel subagents and the active Codex environment supports them, the validation passes may be split, but the default path is single-threaded.

## Reference Files

Load only what is needed:

- Taxonomy: `references/ai-tell-taxonomy.md`
- Rewriting playbook: `references/rewriting-playbook.md`
- Author context schema: `references/author-context-schema.md`
- Pattern candidates baseline: `references/pattern-candidates.md`
- Sample collection and promotion rules: `references/sample-collection.md`, `references/promotion-checklist.md`
- Web service spec, only for web/API requests: `references/web-service-spec.md`
- Original Claude role prompts, for role behavior only: `references/agents/*.md`

Treat these as read-only when the skill is installed from a plugin cache.

## Phase 0: Context

1. Determine the current working directory.
2. Create or select `run_id` as `YYYY-MM-DD-NNN`.
3. New text or no `_workspace/` means a new run.
4. Follow-up requests such as "이 문단만 다시", "번역투만 더", "강도 낮춰", or "2차 윤문" should reuse the latest run under `_workspace/`.
5. Look for an optional voice profile at:
   - `<cwd>/_workspace/{run_id}/author-context.yaml`
   - `<cwd>/author-context.yaml`
6. If `author-context.yaml` exists, validate it against `references/author-context-schema.md`.
7. If validation fails, reject the whole voice profile and explain the concrete failing field. Do not silently fall back.
8. If validation passes, record applied settings in `_workspace/{run_id}/voice_profile_log.json`.

## Phase 1: Input

1. Accept pasted text or a `.txt` or `.md` file path.
2. Save the original text exactly as `_workspace/{run_id}/01_input.txt`.
3. Estimate genre from the first 300 characters unless the user specified it.
4. Default options:
   - `min_severity: S2`
   - `include_document_level: true`
   - rewrite strength: `기본`

If input is under 100 Korean characters, continue but mark confidence as low.

## Phase 2: Detection

Act as `ai-tell-detector` using the taxonomy.

Produce `_workspace/{run_id}/02_detection.json` with:

- `meta.run_id`
- `meta.input_length`
- `meta.estimated_genre`
- `meta.detected_count`
- `meta.ai_tell_density`
- `meta.severity_weighted_score`
- `meta.category_summary`
- `findings[]`

Each finding should include:

- `id`
- `category`
- `category_label`
- `severity`
- `scope`
- `text_span` when span-based
- `start` and `end` offsets when practical
- `reason`
- `suggested_fix`

Detection priorities:

- A: translationese
- B: excessive English or parenthetical terms
- C: mechanical structure, headings, bullets, emoji
- D: AI cliches and hype wording
- E: rhythm uniformity
- F: redundant modifiers
- G: hedging overuse
- H: sentence-initial connectors
- I: formal nouns and empty phrasing
- J: visual decoration

If no meaningful findings are detected, stop with "AI 티가 거의 없습니다. 윤문 불필요" and still save a short summary.

## Phase 3: Rewrite

Act as `korean-style-rewriter` using `02_detection.json` and the playbook.

Write:

- `_workspace/{run_id}/03_rewrite.md`
- `_workspace/{run_id}/03_rewrite_diff.json`

Rewrite order:

1. D cliches
2. A translationese
3. I formal nouns
4. G hedging and possibility wording
5. H sentence-initial connectors
6. F redundant modifiers
7. B excessive English
8. C and J structure or decoration
9. E rhythm

`03_rewrite_diff.json` should include:

- `char_count_before`
- `char_count_after`
- `change_rate`
- `findings_resolved`
- `findings_unresolved`
- `over_polish_warning`
- `edits[]` with `finding_id`, `before`, `after`, `category`, and `reason`

Never add new claims, examples, facts, metaphors, or opinions.

## Phase 4: Verification

Run two independent review passes.

### Content Fidelity Audit

Act as `content-fidelity-auditor`.

Compare `01_input.txt`, `03_rewrite.md`, and `03_rewrite_diff.json`.

Write `_workspace/{run_id}/04_fidelity_audit.json` with:

- `audit_verdict`: `full_pass`, `conditional_pass`, or `fail`
- `total_edits`
- `edits_passed`
- `edits_flagged`
- `rollback_required`
- `flagged_edits[]`

Rollback or flag any change that affects:

- names
- numbers
- dates
- direct quotes
- legal or regulatory text
- formulas
- claim direction
- causality
- subject identity
- quantifiers
- negation
- ordering
- omission or addition

### Naturalness Review

Act as `naturalness-reviewer`.

Review the rewrite without applying voice profile overrides. This keeps one external-looking quality layer.

Write `_workspace/{run_id}/05_naturalness_review.json` with:

- `score_before`
- `score_after`
- `score_improvement`
- `s1_residual`
- `s2_residual`
- `over_polish_signals`
- `verdict`: `accept`, `accept_with_note`, `rewrite_round_2`, `rollback_and_rewrite`, or `hold_and_report`
- `quality_level`: `A`, `B`, `C`, or `D`
- `residual_findings[]`
- `over_polish_findings[]`

If new suspicious patterns are found, write them to `_workspace/{run_id}/06_pattern_candidates.md`. Do not edit `references/pattern-candidates.md`.

## Phase 5: Decision

Combine the audit and review:

- `full_pass` plus `accept` or `accept_with_note`: final approval.
- `full_pass` plus `rewrite_round_2`: rewrite only target findings.
- `conditional_pass`: rollback or revise flagged edits only.
- `fail`: restart rewrite from Phase 3.
- Serious over-polish or three failed rounds: `hold_and_report`.

Maximum rewrite rounds: 3.

Use versioned files for additional rounds:

- `03_rewrite_v2.md`
- `03_rewrite_diff_v2.json`
- `04_fidelity_audit_v2.json`
- `05_naturalness_review_v2.json`

## Phase 6: Final Output

Write:

- `_workspace/{run_id}/final.md`
- `_workspace/{run_id}/summary.md`

Reply to the user with:

- the rewritten text
- quality grade
- score or finding count change
- 3 to 5 key changes, each grounded in a finding
- any unresolved risks or residual patterns
- the run artifact path

If grade is B or lower, mention that a focused second pass can target the remaining issues.

## Follow-Up Commands In Natural Language

Codex does not need Claude slash commands. Interpret these naturally:

- "이 문단만 다시 윤문해줘"
- "번역투만 더 손봐줘"
- "관용구만 다시"
- "윤문 강도 낮춰줘"
- "원문 톤을 더 살려줘"
- "2차 윤문해줘"

Find the latest run under `_workspace/` and continue from Phase 3 or Phase 4 as needed.

## Web Service Mode

If the user asks to build a web service, API, product, or deployment around this workflow, load `references/web-service-spec.md` and produce architecture notes first. Do not implement a web app unless the user asks for implementation.
