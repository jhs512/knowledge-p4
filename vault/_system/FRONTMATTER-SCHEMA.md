# Frontmatter 스키마

모든 노드는 아래 frontmatter를 가집니다. ✅ = 필수, ⬜ = 선택.

```yaml
---
id: decision-work-first          # ✅ 파일명(확장자 제외)과 일치
title: "첫 결정"                  # ✅ 사람이 읽는 제목
type: decision                   # ✅ NODE-TYPES.md의 16종 중 하나
namespace: work                  # ✅ 최상위 영역 (기본값: work)
visibility: public               # ✅ public | private | sensitive (기본값: public)
confidence: 0.9                  # ✅ 0.0~1.0 — 확신 정도. 시간 경과로 감쇠됨
created: 2026-06-10              # ✅ 생성일 (YYYY-MM-DD)
updated: 2026-06-10              # ✅ 마지막 수정일 — 내용 변경 시 반드시 갱신
summary: "한 줄 요약"             # ✅ 본문을 열지 않고도 파악 가능한 1문장
edges:                           # ✅ typed edge 목록 (자리 잡은 노드는 비우지 말 것)
  - type: supports               #    EDGE-TYPES.md의 10종 중 하나
    target: pillar-work-foundation  # 대상 노드의 id
tags: []                         # ⬜ 자유 태그 (Obsidian 호환)
status: active                   # ⬜ task 등에서 사용: active | done | blocked | archived
aliases: []                      # ⬜ 별칭 (Obsidian 호환)
source_url: ""                   # ⬜ source/bookmark 타입의 원본 URL
---
```

## 필드 규칙

- **id**: 소문자 케밥 케이스, `<type>-<namespace>-<slug>` 형식 권장.
- **confidence**: fact·검증된 decision은 0.8 이상, hypothesis는 0.3~0.6에서 시작. `/vault-health`가 오래된 노드의 confidence를 감쇠시킨다.
- **summary**: `/query-vault`가 본문 대신 읽는 필드 — 토큰 절약의 핵심이므로 성실히 작성.
- **edges**: 형식은 [EDGE-TYPES.md](EDGE-TYPES.md) 참조.
- **visibility**: 의미는 [AGENTS.md](AGENTS.md)의 visibility 모델 참조.
- 날짜는 항상 절대 표기(YYYY-MM-DD). 상대 표기("어제") 금지.
