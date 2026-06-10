---
id: decision-work-first
title: "지식 관리를 Infinite Brain vault로 한다"
type: decision
namespace: work
visibility: public
confidence: 0.9
created: 2026-06-10
updated: 2026-06-10
summary: "업무 지식을 typed node/edge 기반의 Infinite Brain vault(vault/)로 관리하기로 결정."
edges:
  - type: supports
    target: pillar-work-foundation
tags: [meta]
---

# 지식 관리를 Infinite Brain vault로 한다

## 결정

업무 지식을 평면적인 노트 더미가 아니라, 타입이 지정된 노드와 엣지로 이루어진 지식 그래프(Infinite Brain)로 관리한다. vault 위치는 저장소의 `vault/` 하위 폴더.

## 근거

- typed edge 덕분에 에이전트가 전체를 읽지 않고 그래프를 선택적으로 탐색할 수 있어 토큰 비용이 크게 줄어든다 (`/query-vault`).
- 원자적 노드 + 명시적 모순 추적(`contradicts`)으로 지식의 신뢰도를 관리할 수 있다.
- Obsidian과 호환되는 평문 마크다운이라 잠금(lock-in)이 없다.

## 결과

이 결정은 [[pillar-work-foundation]]을 뒷받침하는 첫 번째 노드다.
