---
id: pattern-work-context-mode-build-redirect
title: "context-mode 훅의 빌드 명령 리다이렉트"
type: pattern
namespace: work
visibility: public
confidence: 0.9
created: 2026-06-10
updated: 2026-06-10
summary: "Bash로 gradlew/mvn/npm 등 빌드 명령 실행 시 context-mode PreToolUse 훅이 차단하고 ctx_execute로 안내함. 오류·장애가 아닌 정상 정책이므로 고치거나 우회할 대상 아님 — 처음부터 ctx_execute(shell) + grep 필터로 실행하면 차단 왕복 1회를 아낌."
edges:
  - type: supports
    target: pillar-work-foundation
  - type: related_to
    target: pattern-work-windows-git-bash
tags: [claude-code, context-mode, tooling, build]
---

# context-mode 훅의 빌드 명령 리다이렉트

## 현상

Bash 도구로 `./gradlew build` 같은 빌드 명령 실행 시 아래 메시지로 차단됨:

> `context-mode: Build tool redirected. Call mcp__plugin_context-mode_context-mode__ctx_execute(...)`

## 이건 오류가 아니다

context-mode 플러그인의 **PreToolUse 훅**이 빌드 도구 명령(gradle/maven/npm 등)을 감지해 Bash 실행을 거부하고 올바른 도구를 안내하는 것. **의도된 컨텍스트 절약 정책**이며 빌드 실패도, 고치거나 우회할 장애도 아니다 — 장황한 빌드 로그가 대화 컨텍스트에 들어가는 것을 막고, 샌드박스에서 실행 후 필요한 줄만 반환하게 한다.

훅 메시지를 받은 뒤 안내대로 따라가도 결과는 같다. 이 문서의 가치는 "해결법"이 아니라 **차단→재시도 왕복 1회를 생략**하는 데 있다.

## 기본 워크플로 (대처 아님)

빌드·테스트·장문 출력 명령은 처음부터 ctx_execute로:

```
ctx_execute(language: "shell",
  code: "cd back2 && ./gradlew build --no-daemon 2>&1 | grep -iE '(^e:|error|FAILED|BUILD)' | head -20",
  timeout: 600000)
```

- `grep -E '(error|FAIL|BUILD)'` 또는 `tail -30`으로 필요한 부분만 출력
- Gradle/Maven은 `timeout` 명시 (기본 RPC 타임아웃보다 길게)
- 셸은 Git Bash 기준 Unix 문법, 경로는 `/c/...` 절대경로 — 환경 전반은 [[pattern-work-windows-git-bash]] 참고
