---
id: log-organize-vault-20260610-115027
title: "organize-vault: 전체 감사 + 5건 개선"
type: custom
namespace: work
visibility: public
confidence: 0.9
created: 2026-06-10
updated: 2026-06-10
summary: "12노드 감사: 타입 위반 1, 빈 edges 1, 200자 초과 summary 1, 크로스링크 3건 발견 — 전부 수정"
operation: organize-vault
affected_nodes:
  - log-query-vault-20260610-112311
  - playbook-work-springboot-init
  - pattern-work-context-mode-build-redirect
  - pattern-work-windows-git-bash
edges:
  - type: derived_from
    target: pattern-work-windows-git-bash
tags: [log, organize-vault]
---

# organize-vault 로그

12노드 감사. 발견: ① `type: log` 비표준 타입(112311) → custom, ② 같은 노드 `edges: []` → derived_from 추가, ③ playbook summary 200자 초과 → 축약, ④ 패턴·플레이북 간 크로스링크 부재 → related_to 3건 추가, ⑤ Windows Git Bash 셸 오류(zip/tar, 상대경로 cd, CRLF)가 미문서화 → `pattern-work-windows-git-bash` 신규 작성, context-mode 패턴은 "오류 아님·기본 워크플로"로 재구성. 모순·스테일·visibility 문제 없음.
