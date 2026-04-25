# Release Notes

이 repo는 원본 `epoko77-ai/im-not-ai`의 버전을 그대로 따라갑니다. `im-not-ai-codex v1.3.1`은 원본 `im-not-ai v1.3.1`을 Codex plugin/skill 구조로 옮긴 배포판입니다.

## v1.3.1 — Codex plugin port for im-not-ai v1.3.1

Codex 포트 변경:

- Codex plugin manifest version을 `1.3.1`로 설정.
- `humanize-korean` Codex skill 추가.
- 원본 `v1.3.1` taxonomy, rewriting playbook, schema, role reference를 plugin 내부에 보존.
- Codex marketplace metadata와 설치 안내 추가.
- 원본 Claude Code edition과 분리된 community plugin 배포판으로 문서화.

원본 기준:

- Original repo: https://github.com/epoko77-ai/im-not-ai
- Original release: https://github.com/epoko77-ai/im-not-ai/releases/tag/v1.3.1
- Original base commit used for this port: `2fc2e3b62dcee40af4d9b7d239ae0a7a463b2cee`

### Original v1.3.1 Notes

v1.3 발행 직후, 사용자께서 직접 Gemini API 키를 제공해 회차 3 진짜 외부 데이터 검증을 진행한 결과를 hotfix patch로 반영합니다. v1.x 발행 정책의 "외부 의존 데이터 도착 시 hotfix patch" 패턴 준수.

#### 본진 신규 2건 (Gemini-우세 시그니처)

- **C-10 콜론 부제 헤딩 공식** "X: Y" 또는 "X: A에서 B로" — 8회·3파일·3도메인
- **D-7 변환 공식** "X에서 Y로 / X을 넘어 Y로" — 7회·2파일·2도메인

#### 본진 보강 3건

- **D-4** Gemini hype 어휘 셋 추가 (압도적·막강한·폭발적·파격적·대대적·강력한)
- **J-2** 빈도 임계 명시 (한 문서 따옴표 5회 초과 시 S2 강화)
- **I-4** 권고형 결말 변종 추가 (~해야 한다·~해야 합니다)

#### 회차 2 hold 후보 검증 — GPT vs Gemini 모델 차이 발견

| 회차 2 hold | GPT | Gemini |
|---|---|---|
| "결국" 문두 | 9+ | 1 |
| "A 아니라 B" | 7+ | 2 |
| 5+ 콤마 나열 | 4 | 0 |

회차 2 hold 후보 3건이 Gemini에서 거의 재현되지 않은 사실은 분류 체계에 "모델 우세 분포" 메타데이터 도입 필요성을 시사합니다(v1.4 검토). hold 3건 모두 status_reason 갱신해 hold 유지.

#### 회차 1·2·3 누적

| 회차 | 데이터 | 본진 신규 | 본진 보강 |
|------|------|---------|---------|
| 1 | Claude 합성 2건 | C-9 | I-2 |
| 2 | 뉴스핌 GPT 2건 | 0 | I-3·H-3 |
| 3 | Gemini 직접 4건 | C-10·D-7 | D-4·J-2·I-4 |
| **합계** | 8건 | **3건** | **6건** |

v1.2 신규 0건 정체기를 v1.3 인프라 + 외부 모델 데이터 3종으로 깬 결과.

#### 데이터 보존

회차 3 Gemini 호출 스크립트(자연 prompt 4종)·출력 본문·manifest는 `_workspace/v1.3-pilot/round3-gemini/`에 로컬 보존(gitignored). 분석 노트는 `04_round3_gemini-analysis.md`.

#### 하위 호환성

v1.3.1은 v1.3과 하위 호환. 운영 인프라 변경 없음, voice profile 동작 동일. 본진 신규 2건·보강 3건은 다음 humanize-korean run부터 자동 적용.

#### PR

[#10](https://github.com/epoko77-ai/im-not-ai/pull/10) — v1.3 → v1.3.1 hotfix 단일 commit

## v1.3 — 서브 패턴 발굴 운영 체계 + 본진 신규 1건·보강 3건

Original release: https://github.com/epoko77-ai/im-not-ai/releases/tag/v1.3

분류 체계의 본진 패턴은 그대로 유지하면서, **에이전트들이 실전에서 만난 미분류 의심 패턴을 단일 풀에 누적·점검·승격하는 운영 인프라**를 도입합니다. v1.1까지의 패턴 추가가 사람의 1회성 결정이었고 v1.2에서 voice profile 권한 위계가 들어왔다면, v1.3은 **분류 체계 자체가 시간을 따라 자라나는 구조**입니다.

발행 전 두 회차의 파일럿(회차 1 합성 샘플 인프라 검증 + 회차 2 외부 매체 진짜 GPT 출력 분석)에서 본진 신규 1건과 보강 3건을 동시에 영구 반영했습니다.

### 운영 인프라 5종

- **`references/pattern-candidates.md`** — 본진 승격 전 모든 의심 패턴을 누적하는 단일 그릇
- **3개 에이전트 풀 적재 채널** — detector·rewriter·naturalness-reviewer가 미분류 패턴을 풀에 직접 적재
- **taxonomist 풀 운영자 역할** — 4가지 trigger 기반 정기 점검, 6단계 절차
- **`references/sample-collection.md`** — 4축 다양성 매트릭스, 4종 채널, 익명화·저작권 5대 정책
- **`references/promotion-checklist.md`** — 6게이트 정량 판정 표준

### 누적 결과

| 항목 | 회차 1 | 회차 2 | 합계 |
|------|-------|-------|------|
| 본진 신규 | C-9 (숫자 괄호 인덱싱) | 0 | **1건** |
| 본진 보강 | I-2 (결합형) | I-3 (결말 변종) · H-3 (메타 진입 변종) | **3건** |
| 풀 hold 누적 | 1건 | 3건 (강력 후보) | **4건** |

### 회차 2 핵심 발견

뉴스핌 [AI로 읽는 경제] 시리즈(ChatGPT 작성 명시) 분석에서 **Gate 1.3 분산 보호장치가 진짜 외부 데이터에서도 정확히 작동**한 것이 가장 큰 결과입니다. 9회+ 등장한 "결국" 문두 단언, 7회+ 등장한 "X은 A가 아니라 B다" 부정-긍정 대구 결산이 occurrences·source distinct는 모두 통과했지만 같은 모델·같은 기자 시리즈라는 정성 분산 검사로 hold 처리됐습니다. 단일 출처 노이즈가 본진을 오염시키지 않으면서 다음 회차에 다른 모델 데이터에서 재현되면 즉시 promoted 가능한 강력 후보로 풀에 누적됐습니다. v1.2 워크플로였다면 이 정보는 모두 묻혔을 것입니다.

### 보너스 발견

본진 H-1 명시 어휘("또한·따라서·즉·나아가·아울러·게다가·더욱이")가 한국 매체 GPT 출력에서 적게 나오고, 본진 미수록 "결국·다시 말해·특히"가 압도적으로 많다는 분포 미스매치 발견. 향후 H-1 어휘 셋의 재캘리브레이션 신호.

### 하위 호환성

v1.3은 v1.2와 하위 호환입니다. 에이전트 입출력 변경 없음, voice profile 동작 동일. 풀 적재는 부수 효과이며 적재 실패가 메인 파이프라인을 막지 않습니다.

### 회차 3 권장 입력 (v1.3.1 hotfix 트리거)

회차 2 데이터 확보의 정직한 한계: Gemini가 작성한 한국 매체 칼럼 본문은 검색에서 직접 인용 텍스트로 확보하지 못했습니다. 회차 3는 다음 데이터 우선 확보 — Gemini Pro 2.5 / HyperCLOVA X·Solar·Exaone / 다른 매체 GPT 출력. 위 3건 중 2건 이상에서 hold 후보가 재현되면 자동 승격 가능. v1.2 정책처럼 회차 3 결과는 v1.3.1 hotfix로 분리 발행 예정.

### PR

[#9](https://github.com/epoko77-ai/im-not-ai/pull/9) — 8 커밋 누적 (인프라 5 + 회차 1 + 회차 2)

### 출처 (회차 2 외부 데이터)

- [뉴스핌 AI로 읽는 경제 ① "AI는 일자리를 없애는가"](https://www.newspim.com/news/view/20260423001060)
- [뉴스핌 AI로 읽는 경제 ② "한국 AI 일자리의 미래"](https://www.newspim.com/news/view/20260423001058)

## v1.2 — 작가 voice profile + 권한 위계

Original release: https://github.com/epoko77-ai/im-not-ai/releases/tag/v1.2

**Issue #1(@simonsez9510) 후속 첫 릴리스.** 외부 contributor의 8.5만 자 단행본 비소설 적용 후기에서 시작해, 그분의 어댑터 reference PR(#3)을 거쳐 메인테이너 schema에 흡수하는 흐름으로 v1.2를 정리했습니다.

코드 변경은 거의 없습니다. **voice profile 미주입 시 v1.1과 100% 동일 동작**(하위 호환). 신기능을 쓰려면 `author-context.yaml`을 명시적으로 작성해 작업 cwd 또는 `_workspace/{run_id}/`에 두면 됩니다.

### 핵심 변경

- **권한 위계 §1~§6 신설** (`ai-tell-taxonomy.md`) — 객관 분류 우선, voice profile은 opt-in, 무력화 불가 패턴(A-8/C-5/D-1~D-6) 영구 default-on, `naturalness-reviewer` 분리 검증층 보존, 회귀 게이트 정책
- **`author-context.yaml` 스키마** (`references/author-context-schema.md`) — 패턴 ID on/off + 임계 완화(multiplier) + Do-NOT 키워드 화이트리스트만 허용. 자유 텍스트 mandate는 schema validator가 거부
- **Multiplier 캡** — 일반 ≤ 2.0, D-1~D-6 ≤ 1.5, A-8·C-5 = 1.0 고정 (임계 우회를 통한 사실상 무력화 방지)
- **Schema validator 책임 강화** — 무력화 불가 disable 거부, 캡 위반 거부, prompt injection escape character 검증, silent fallback 금지(파일 거부 시 명시 에러)
- **`reviewer_contract.naturalness_reviewer_voice_blind: true`** 강제 필드 — §5 분리 검증층을 schema 단계에서 contract로 잠금
- **Telemetry** — `voice_profile_log.json` 발행 (적용·거부·trigger 키워드 추적, §6 회귀 게이트 measurable 입력)
- **에이전트 주입 분리** — `ai-tell-detector`·`korean-style-rewriter`·`content-fidelity-auditor` 주입, `naturalness-reviewer` 의도적 미주입
- **경로 토큰화** — `SKILL.md`의 절대 경로 제거, `_workspace/`는 cwd 기준 (글로벌 설치 지원)
- **다운스트림 caller reference** — `references/proposals/` (PR #3 어댑터 reference 격리 보존, 메인테이너 SSOT 외부)

### 사용 예시 (단행본 비소설 작가)

```yaml
# author-context.yaml — 작업 cwd 또는 _workspace/{run_id}/에 명시 배치
version: "1.0"

profile:
  author: "Won Seongmuk"
  work: "단행본 비소설 (8.5만 자)"
  notes: "단단한 서술체, em-dash 리듬 장치"

pattern_overrides:
  - id: "J-3"
    action: "relax"
    multiplier: 2.0
  - id: "A-10"
    action: "disable"

do_not_extra:
  - "1인칭 진입"

reviewer_contract:
  naturalness_reviewer_voice_blind: true
```

### 회귀 검증 (정직성 노트)

v1.2 본체는 코드 변경이 거의 없어 회귀 위험이 낮습니다(voice profile 미주입 모드 = v1.1 100% 동등). 그러나 **외부 회귀 케이스 검증은 아직 진행되지 않은 self-reported 상태**입니다. 답글에서 약속한 외부 케이스 2~3건 검증은 v1.2.1 hotfix로 반영할 예정이며, 외부 케이스 모집 Issue가 별도로 열릴 예정입니다(@simonsez9510 진행).

v1.1 self-dogfooding 정직성 톤을 유지하기 위해, 외부 검증 부재를 release notes에 명시 표기합니다.

### 외부 contributor

- [@simonsez9510](https://github.com/simonsez9510) — [Issue #1](https://github.com/epoko77-ai/im-not-ai/issues/1) 8.5만 자 단행본 비소설 적용 후기 + 개선 제안 4건 + [PR #3](https://github.com/epoko77-ai/im-not-ai/pull/3) 어댑터 reference (multiplier 캡·`reviewer_contract` 강제·telemetry·prompt injection 방어 통찰)

### 분량

5 commits, 8 files changed, +441 / -12

### 마이그레이션

v1.1 사용자는 별도 작업 없이 v1.2로 자동 전환됩니다(하위 호환). voice profile을 쓰려면 `references/author-context-schema.md`를 참고해 `author-context.yaml`을 작성해 두십시오.
