# Humanize Korean for Codex

[epoko77-ai/im-not-ai](https://github.com/epoko77-ai/im-not-ai)의 Codex 플러그인 포트입니다. 공식 upstream `v1.3.1`, commit `2fc2e3b62dcee40af4d9b7d239ae0a7a463b2cee` 기준으로 만들었습니다.

`im-not-ai`는 한글 AI 글에서 자주 보이는 번역투, 기계적 병렬, 관용구, 접속사 남발, 리듬 균일성 등을 탐지한 뒤, **의미·수치·고유명사·날짜·직접 인용은 보존하고 문체만 윤문**하는 워크플로입니다.

## 상태

이 repo는 공식 Claude Code 스킬이 아니라, 같은 taxonomy/playbook을 Codex plugin/skill 구조로 옮긴 community adapter입니다.

- 원본 프로젝트: `epoko77-ai/im-not-ai`
- Codex plugin name: `im-not-ai-codex`
- Codex skill name: `humanize-korean`
- Plugin version: `0.1.0`
- Upstream content version: `v1.3.1`

## 설치

GitHub에 이 repo를 올린 뒤 Codex plugin marketplace로 추가합니다.

```bash
codex plugin marketplace add Poo-Squirry/im-not-ai-codex
```

그 다음 Codex를 재시작하고, Plugins 화면에서 `im-not-ai Codex`를 선택한 뒤 `Humanize Korean`을 설치하면 됩니다.

로컬에서 먼저 테스트하려면:

```bash
codex plugin marketplace add /absolute/path/to/im-not-ai-codex
```

Codex plugin/marketplace 구조는 공식 문서를 기준으로 맞췄습니다: https://developers.openai.com/codex/plugins/build

## 사용법

Codex에서 자연어로 부르면 됩니다.

```text
humanize-korean으로 이 글 AI 티 없애줘:

[윤문할 한글 초안]
```

트리거 예시:

- `AI 티 없애줘`
- `GPT 문체 제거해줘`
- `사람이 쓴 것처럼 윤문해줘`
- `번역투 제거`
- `한글 AI 윤문`
- `humanize-korean으로 윤문해줘`

스킬은 현재 작업 폴더에 `_workspace/{YYYY-MM-DD-NNN}/`를 만들고 산출물을 저장합니다.

- `01_input.txt`: 원문
- `02_detection.json`: AI 티 탐지 리포트
- `03_rewrite.md`: 윤문본
- `03_rewrite_diff.json`: 변경 diff
- `04_fidelity_audit.json`: 의미 보존 감사
- `05_naturalness_review.json`: 자연스러움 리뷰
- `final.md`: 최종 윤문본
- `summary.md`: 요약 리포트

## Claude Code 버전과 다른 점

원본은 Claude Code 프로젝트 구조를 씁니다.

- `.claude/skills/humanize-korean/SKILL.md`
- `.claude/agents/*.md`
- `.claude/commands/humanize.md`
- `.claude/commands/humanize-redo.md`

이 포트는 원본의 taxonomy, playbook, schema, agent role reference는 유지하되 Codex plugin 구조로 감쌌습니다.

- `.codex-plugin/plugin.json`
- `.agents/plugins/marketplace.json`
- `skills/humanize-korean/SKILL.md`
- `skills/humanize-korean/references/`

Claude Code의 `/humanize`, `/humanize-redo` 슬래시 커맨드는 Codex에서 그대로 쓰지 않습니다. 대신 `humanize-korean` 스킬 트리거로 실행합니다.

## 원본 출처

Original project by [@epoko77-ai](https://github.com/epoko77-ai), licensed under MIT. 이 포트도 MIT License를 유지하며, 원본 reference 파일은 그대로 보존하고 Codex adapter layer만 추가했습니다.

정확한 source commit과 파일 매핑은 [UPSTREAM.md](./UPSTREAM.md)를 참고하세요.

## Privacy

이 플러그인은 Codex skill instruction과 Markdown reference 파일만 포함합니다. 별도 MCP 서버, 앱 connector, 외부 API 호출, telemetry endpoint, background service를 추가하지 않습니다.

단, Codex 자체는 사용자의 Codex/OpenAI 설정에 따라 프롬프트를 처리할 수 있습니다. 민감한 글은 현재 Codex workspace와 계정 설정이 적절한지 확인한 뒤 넣으세요.

## Terms

이 community port는 MIT License로 제공됩니다. OpenAI 공식 플러그인이 아니며, `epoko77-ai/im-not-ai`의 공식 Claude Code edition도 아닙니다.

## Upstream README PR 제안

원본 README에 올릴 때는 아래처럼 작은 community 링크 섹션만 추가하는 방식이 좋습니다.

```md
### 방법 D - Codex Plugin (community)

Codex Desktop/CLI에서 사용할 수 있는 community plugin 포트입니다.

- Repo: https://github.com/Poo-Squirry/im-not-ai-codex
- 원본 taxonomy/playbook을 유지하고 Codex plugin/skill 구조에 맞게 어댑터화했습니다.
- 공식 Claude Code 버전과 별도로 유지됩니다.
```
