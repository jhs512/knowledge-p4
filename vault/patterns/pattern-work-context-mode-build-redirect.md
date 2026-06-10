---
id: pattern-work-context-mode-build-redirect
title: "context-mode 훅의 빌드 명령 리다이렉트"
type: pattern
namespace: work
visibility: public
confidence: 0.9
created: 2026-06-10
updated: 2026-06-10
summary: "Bash로 gradlew/mvn/npm 등 빌드 명령 실행 시 context-mode PreToolUse 훅이 차단하고 ctx_execute 사용을 요구함. 오류가 아니라 정책 — 빌드·테스트는 처음부터 ctx_execute(shell) + grep 필터로 실행할 것."
edges:
  - type: supports
    target: pillar-work-foundation
tags: [claude-code, context-mode, tooling, build]
---

# context-mode 훅의 빌드 명령 리다이렉트

## 증상

Bash 도구로 `./gradlew build` 같은 빌드 명령 실행 시 아래 에러로 차단됨:

> `context-mode: Build tool redirected. Call mcp__plugin_context-mode_context-mode__ctx_execute(...)`

## 원인

context-mode 플러그인의 **PreToolUse 훅**이 빌드 도구 명령(gradle/maven/npm 등)을 감지해 Bash 실행을 거부하는 것. 실제 빌드 실패가 아니라 **컨텍스트 절약 정책** — 장황한 빌드 로그가 대화 컨텍스트에 들어가는 것을 막고, 샌드박스에서 실행 후 필요한 줄만 반환하게 함.

## 대처

빌드·테스트·장문 출력 명령은 Bash로 시도하지 말고 처음부터 ctx_execute로:

```
ctx_execute(language: "shell",
  code: "cd back2 && ./gradlew build --no-daemon 2>&1 | grep -iE '(^e:|error|FAILED|BUILD)' | head -20",
  timeout: 600000)
```

- `grep -E '(error|FAIL|BUILD)'` 또는 `tail -30`으로 필요한 부분만 출력
- Gradle/Maven은 `timeout` 명시 (기본 RPC 타임아웃보다 길게)
