<img src="https://raw.githubusercontent.com/epoko77-ai/im-not-ai/main/assets/social-preview.png" alt="im-not-ai — 한글 AI 티 제거기 v2.0" width="820">

> **한국 번역학자의 책상에서 가져온 분류 체계** — 영-한 번역학계 8유형 + post-editese 정량 트랙. monolith·5인 정의 무수정.

## v2.0.0 (2026-05-07)

한국 번역학계 8대 번역투 유형(이근희·김정우·김도훈·곽은주·김순영·박옥수·김혜영·이영옥) + Toral 2019 post-editese 3축 통합. monolith·5인 정의 무수정, 도구 호출 3회 캡(v1.6.1) 보존.

### 신규 4건 (본진 등재)
- A-16 영어 대명사 직역 [S1]
- A-18 관계대명사절 좌향 수식 [S2]
- A-19 이중 조사 결합 [S2]
- E-7 청자 경어법 일관성 손실 [S2 estimated, dialogue 가드]

### Hold 1건 (v2.0b — NMT 원본 회차 후 v2.1 부활 대기)
- A-17 무정물·추상명사 '-들' — v1.6 5편 + 외부 회차 위키 6편 양성 0건. 학술 anchor·metric·scholarship.md §4 보존, ID 슬롯 비워둠.

### 보강 4건
- A-15 사역·인지 동사 분리 / A-7 light verb construction 일반화 / F-4 영어 명사화 4종 통합 / E-2 진행형 자동 매핑

### post-editese 14 metric (metric-only 트랙, caveat C3)
simplification 3 + normalisation 2 + interference 8 + interference_index 합성 = 14. 본진 패턴 ID 미부여, baseline_v2.json 70셀 placeholder.

### 회귀 검증
- v1.6 5편 점수 산출 (재윤문 없음): 회귀 0건, lexical_diversity 5편 전수 상승.
- 외부 회차 위키 6편: A-16 양성 50%·A-18 양성 67%, interference_index 외부 0.251 vs v1.6 0.05~0.10 (Toral 가설 1차 부합).

### 호환성
- 슬래시 커맨드 무변경. monolith 헤더 +0.6KB. patternID 안정성 ✓.

PR: https://github.com/epoko77-ai/im-not-ai/pull/19
