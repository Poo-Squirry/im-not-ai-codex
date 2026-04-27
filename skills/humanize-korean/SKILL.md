---
name: humanize-korean
version: "1.5.1"
description: Use when the user asks to make Korean AI-generated writing sound naturally human while preserving meaning. Trigger phrases include "AI 티 없애줘", "GPT 문체 제거", "사람이 쓴 것처럼 윤문", "번역투 제거", "한글 AI 윤문", "AI 글 사람처럼", "ChatGPT 티 제거", "휴머나이저", and "humanize Korean". This Codex skill is a community plugin port of epoko77-ai/im-not-ai v1.5.1 with fast monolith and strict workflows.
---

# Humanize Korean for Codex

Codex plugin port of `epoko77-ai/im-not-ai`. Detect Korean AI-writing tells, rewrite only the affected style spans, then audit that meaning is preserved.

Original source baseline:

- Original repo: `https://github.com/epoko77-ai/im-not-ai`
- Original version label: `v1.5.1`
- Original release tag: none yet; upstream `main` carries the v1.5.1 taxonomy update
- Current upstream commit checked during sync: `f6f208245d689744c2a3a79d74a2f79ba626ef62`

## First Response

When this skill starts, print this line before continuing:

```text
humanize-korean codex-port v1.5.1 / original im-not-ai v1.5.1 / {fast|strict} mode / run_id: {YYYY-MM-DD-NNN}
```

Then continue the workflow in the same response or turn.

## v1.5 Mode Policy

Official v1.5 rolled back the expensive v1.2-v1.4 hot path and made `humanize-monolith` the default.

- **Fast mode (default)**: use the monolith workflow with `references/quick-rules.md`. Best for normal pasted Korean drafts up to about 5,000 characters.
- **Strict mode**: use the 5-role pipeline with the full taxonomy and playbook. Use it when the user says `--strict`, asks for "정밀 모드" or "5인 파이프라인", input is over 8,000 Korean characters, or the request is a follow-up such as "이 카테고리만 다시" or "2차 윤문".
- Voice profile, candidate pool, promotion checklist, and sample collection references were removed upstream in v1.5 and must not be reintroduced by this Codex port.

This Codex port does not require Claude Code `Agent`, `TeamCreate`, slash commands, or `model: opus`. Run the role behavior directly inside this skill workflow. Treat the original Claude agent prompts in `references/agents/*.md` as read-only behavior references.

## Core Rules

1. Meaning is sacred. Facts, claims, dates, numbers, names, products, institutions, legal text, formulas, and direct quotes must stay intact.
2. Rewrite only style, rhythm, expression, and structure that are grounded in detected findings.
3. Preserve genre and register. Do not turn a report into an essay, a column into literature, or a formal speech into casual chat.
4. Do not over-polish. Warn over 30 percent change rate. Stop and report over 50 percent change rate.
5. Do not mutate bundled reference files. Write all run artifacts under the current working directory's `_workspace/{run_id}/`.
6. Do not load unrelated project files to infer preferences. Use only the user request, the input text, and this skill's references unless the user points to another file.

## Reference Files

Load only what is needed:

- Fast rules: `references/quick-rules.md`
- Full taxonomy: `references/ai-tell-taxonomy.md`
- Rewriting playbook: `references/rewriting-playbook.md`
- Web service spec, only for web/API requests: `references/web-service-spec.md`
- Original Claude role prompts, for behavior only: `references/agents/*.md`

Treat these as read-only when the skill is installed from a plugin cache.

## Phase 0: Context And Mode

1. Determine the current working directory.
2. Create or select `run_id` as `YYYY-MM-DD-NNN`.
3. New text or no `_workspace/` means a new run.
4. Follow-up requests such as "이 문단만 다시", "번역투만 더", "강도 낮춰", or "2차 윤문" should reuse the latest run under `_workspace/` when appropriate and switch to strict mode.
5. Choose mode:
   - User explicitly asks for `--strict`, "정밀 모드", or "5인 파이프라인": strict.
   - Input is over 8,000 Korean characters: strict and tell the user in one line.
   - Follow-up or partial re-run request: strict.
   - Otherwise: fast.

If input is under 100 Korean characters, continue but mark confidence as low.

## Fast Mode: Monolith Workflow

Use this for the default path. Act as `humanize-monolith` using `references/quick-rules.md` and, if needed, `references/agents/humanize-monolith.md`.

### Phase 1: Input

1. Accept pasted text or a `.txt` or `.md` file path.
2. Save the original text exactly as `_workspace/{run_id}/01_input.txt`.
3. Estimate genre from the first 300 characters unless the user specified it.

### Phase 2: Detect, Rewrite, Self-Verify

In one compact pass:

1. Load `references/quick-rules.md`.
2. Scan for S1 and S2 patterns by category A-J.
3. Apply the Do-NOT list before editing: names, numbers, dates, direct quotes, legal text, formulas, and standard English abbreviations such as LLM, GPU, MCP, and API.
4. Rewrite only matched style spans. Preferred order: D -> A -> I -> G -> H -> F -> B -> C/J -> E.
5. Track edits in memory: `finding_id`, `before`, `after`, category, and reason.
6. Self-check the six v1.5 criteria from `quick-rules.md`: preservation, change rate, genre, register, S1 residuals, and no artificial expressions.
7. Roll back any edit that violates preservation or pushes the change rate too far.

### Phase 3: Fast Artifacts

Write:

- `_workspace/{run_id}/final.md`
- `_workspace/{run_id}/summary.md`

`summary.md` should include:

- original length, rewritten length, change rate
- category counts before and after
- self-check pass count out of 6
- quality grade A/B/C/D
- 3 to 5 grounded highlights
- residual findings, if any

## Strict Mode: Full Role Workflow

Use this when precision matters or the fast path is not appropriate. Run the role behavior directly in Codex, using the original prompts under `references/agents/` as guidance.

### Phase A: Detection

Act as `ai-tell-detector` using `references/ai-tell-taxonomy.md`.

Write `_workspace/{run_id}/02_detection.json` with:

- `meta.run_id`
- `meta.input_length`
- `meta.estimated_genre`
- `meta.detected_count`
- `meta.ai_tell_density`
- `meta.severity_weighted_score`
- `meta.category_summary`
- `findings[]`

Each finding should include `id`, `category`, `category_label`, `severity`, `scope`, `text_span` when span-based, offsets when practical, `reason`, and `suggested_fix`.

### Phase B: Rewrite

Act as `korean-style-rewriter` using `02_detection.json` and `references/rewriting-playbook.md`.

Write:

- `_workspace/{run_id}/03_rewrite.md`
- `_workspace/{run_id}/03_rewrite_diff.json`

Never add new claims, examples, facts, metaphors, or opinions.

### Phase C: Verification

Run two independent review passes.

Content fidelity audit:

- Act as `content-fidelity-auditor`.
- Compare `01_input.txt`, `03_rewrite.md`, and `03_rewrite_diff.json`.
- Write `_workspace/{run_id}/04_fidelity_audit.json`.
- Roll back or flag any change that affects names, numbers, dates, direct quotes, legal text, formulas, claim direction, causality, subject identity, quantifiers, negation, ordering, omission, or addition.

Naturalness review:

- Act as `naturalness-reviewer`.
- Re-check residual AI tells and over-polish signals.
- Write `_workspace/{run_id}/05_naturalness_review.json`.
- If new suspicious patterns are found, write them to `_workspace/{run_id}/06_pattern_candidates.md`. Do not edit bundled references.

### Phase D: Decision

- `full_pass` plus `accept` or `accept_with_note`: final approval.
- `full_pass` plus `rewrite_round_2`: rewrite only target findings.
- `conditional_pass`: rollback or revise flagged edits only.
- `fail`: restart rewrite from Phase B.
- Serious over-polish or three failed rounds: `hold_and_report`.

Maximum rewrite rounds: 3. Use versioned files for additional rounds, such as `03_rewrite_v2.md`.

## Final Output

Write:

- `_workspace/{run_id}/final.md`
- `_workspace/{run_id}/summary.md`

Reply to the user with:

- the rewritten text
- quality grade
- score or finding count change
- 3 to 5 key changes grounded in findings
- unresolved risks or residual patterns
- the run artifact path

If grade is B or lower, mention that `--strict` or a focused second pass can target the remaining issues.

## Follow-Up Commands In Natural Language

Codex does not need Claude slash commands. Interpret these naturally:

- "이 문단만 다시 윤문해줘"
- "번역투만 더 손봐줘"
- "관용구만 다시"
- "윤문 강도 낮춰줘"
- "원문 톤을 더 살려줘"
- "2차 윤문해줘"

Find the latest run under `_workspace/` and continue in strict mode from the relevant phase.

## Web Service Mode

If the user asks to build a web service, API, product, or deployment around this workflow, load `references/web-service-spec.md` and produce architecture notes first. Do not implement a web app unless the user asks for implementation.
