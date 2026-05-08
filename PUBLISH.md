# Publish Checklist

대상 GitHub repo를 `Squirbie/im-not-ai-codex`로 가정한 체크리스트입니다. 다른 repo로 올릴 거면 owner/name만 바꾸면 됩니다.

## 1. GitHub Repo 만들기

빈 public repository를 만듭니다.

```text
https://github.com/Squirbie/im-not-ai-codex
```

이 폴더를 그대로 push할 예정이면 GitHub에서 README나 LICENSE를 자동 생성하지 않는 쪽이 편합니다.

## 2. Plugin Package Push

이 `im-not-ai-codex` 폴더 안에서:

```bash
git init
git branch -M main
git add .
git commit -m "Create Codex plugin port for im-not-ai"
git remote add origin git@github.com:Squirbie/im-not-ai-codex.git
git push -u origin main
```

SSH 설정이 안 되어 있으면 HTTPS remote를 쓰면 됩니다.

```bash
git remote add origin https://github.com/Squirbie/im-not-ai-codex.git
```

## 3. Marketplace 설치 테스트

push 후 Codex plugin marketplace로 추가합니다.

```bash
codex plugin marketplace add Squirbie/im-not-ai-codex
```

Codex를 재시작하고 Plugins 화면에서 `im-not-ai Codex`를 선택한 뒤 `Humanize Korean`을 설치합니다.

로컬에서 먼저 테스트하려면:

```bash
codex plugin marketplace add /absolute/path/to/im-not-ai-codex
```

Marketplace manifest는 `plugins/im-not-ai`를 plugin source로 가리킵니다. Codex CLI 0.128.x에서는 repo root `./`를 plugin source로 쓰면 `/plugins` 목록에서 보이지 않을 수 있으므로, plugin payload는 이 하위 폴더 안에 둡니다.

## 4. Smoke Test

새 Codex 세션에서 이렇게 테스트합니다.

```text
humanize-korean으로 이 글 AI 티 없애줘:

AI 기술을 통해 우리는 다양한 문제를 해결할 수 있을 것으로 보인다. 결론적으로 이는 시사하는 바가 크다.
```

기대 동작:

- `humanize-korean codex-port v2.0.0` 시작 라인이 나온다.
- `_workspace/{run_id}/` 산출물이 생긴다.
- 핵심 의미는 유지하고 문체만 다듬는다.
- 기본 fast mode에서는 `final.md`를 만들고, 파일 끝의 `<!-- HUMANIZE-SUMMARY -->` HTML 주석 블록에 변경률·등급·자체검증 요약을 담는다.

정밀 모드도 확인하려면:

```text
humanize-korean으로 이 글 AI 티 없애줘 --strict:

AI 기술을 통해 우리는 다양한 문제를 해결할 수 있을 것으로 보인다. 결론적으로 이는 시사하는 바가 크다.
```

기대 동작:

- strict mode로 시작한다.
- `02_detection.json`, `03_rewrite.md`, `04_fidelity_audit.json`, `05_naturalness_review.json` 산출물이 생긴다.

## 5. 공식 README 링크 상태

원본 공식 repo의 README community-port 링크는 main에 반영됐습니다.

- PR #12: https://github.com/epoko77-ai/im-not-ai/pull/12
- 상태: merged `2026-04-29`
- 범위: README에 Codex community plugin 링크와 설치 명령 추가
- 의도: 원본 Claude Code 버전은 그대로 두고, Codex 사용자는 이 별도 plugin repo로 안내

원본 README에는 별도 community Web UI 포트도 추가됐습니다.

- PR #15: https://github.com/epoko77-ai/im-not-ai/pull/15
- 상태: merged `2026-04-29`
- 범위: opencode 기반 Web UI 링크 추가

최신 upstream v2.0.0도 반영됐습니다.

- PR #19: https://github.com/epoko77-ai/im-not-ai/pull/19
- PR #20: https://github.com/epoko77-ai/im-not-ai/pull/20
- PR #21: https://github.com/epoko77-ai/im-not-ai/pull/21
- PR #22: https://github.com/epoko77-ai/im-not-ai/pull/22
- PR #23: https://github.com/epoko77-ai/im-not-ai/pull/23
- Release: https://github.com/epoko77-ai/im-not-ai/releases/tag/v2.0.0
- Codex port version: `v2.0.0`
