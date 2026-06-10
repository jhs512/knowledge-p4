# Infinite Brain vault

이 디렉터리는 typed node / typed edge 기반의 지식 그래프 vault입니다.

**모든 vault 작업 전에 `_system/AGENTS.md`를 먼저 읽으세요** — 노드/엣지 분류 체계, visibility 모델, frontmatter 스키마, 금지 사항이 정의되어 있습니다. 마스터 인덱스는 `_system/INDEX.md`입니다.

- 기본 namespace: `work` / 기본 visibility: `public`

| 명령 | 용도 |
|---|---|
| `/convert-note` | `raw/`의 원본 자료를 원자적 노드로 분해 |
| `/query-vault` | 그래프를 선택 탐색하며 질문에 답변 |
| `/organize-vault` | 고아·모순·confidence 격차 대화형 감사 |
| `/vault-health` | confidence 감쇠 + 전체 감사 + 건강 보고서 |
