---
id: pattern-work-windows-git-bash
title: "Windows + Git Bash 셸 환경 주의사항"
type: pattern
namespace: work
visibility: public
confidence: 0.9
created: 2026-06-10
updated: 2026-06-10
summary: "개발 환경은 Windows 11 + Git Bash 기본 셸(USE_POWERSHELL 끔). zip은 tar로 못 풀고(unzip 부재 시 powershell Expand-Archive), 경로는 /c/... 절대경로, CRLF 경고는 무시, Unix 문법 유지."
edges:
  - type: supports
    target: pillar-work-foundation
  - type: related_to
    target: pattern-work-context-mode-build-redirect
tags: [windows, git-bash, shell, tooling, claude-code]
---

# Windows + Git Bash 셸 환경 주의사항

## 환경

- Windows 11 + **Git Bash가 기본 셸** (`USE_POWERSHELL` 계열 설정 끔)
- 명령은 **Unix 문법**으로 작성 (`/dev/null`, 포워드 슬래시). PowerShell/cmd 문법 금지 — 단, 아래 예외처럼 `powershell -Command`를 도구로 호출하는 건 가능

## 실제로 겪은 오류와 대처

### 1. zip 해제 — `tar: This does not look like a tar archive`

Git Bash의 tar는 **GNU tar**라 zip을 못 푼다 (Windows 네이티브 `tar.exe`(libarchive)는 가능하지만 bash PATH에선 GNU tar가 잡힘). `unzip`도 기본 미설치.

```bash
# 폴백 체인 — 이 순서로 시도
unzip -q file.zip -d . 2>/dev/null || powershell -NoProfile -Command "Expand-Archive -Path file.zip -DestinationPath . -Force"
```

### 2. 상대경로 cd 실패 — `cd: knowledge: No such file or directory`

Bash 도구의 **작업 디렉터리는 호출 간 유지됨** (이전에 `cd back2` 했으면 거기서 시작). 상대경로 cd는 깨지기 쉬움.

```bash
# 항상 절대경로 (Git Bash 표기)
cd /c/Users/jangk/projects/p5/knowledge
```

### 3. git CRLF 경고 — `LF will be replaced by CRLF`

경고일 뿐 실패 아님. core.autocrlf 동작이며 무시해도 됨.

## 일반 원칙

- 빌드/장문 출력 명령은 [[pattern-work-context-mode-build-redirect]] 참고 — ctx_execute로
- Windows 전용 도구가 꼭 필요할 때만 `powershell -NoProfile -Command "..."` 단발 호출
