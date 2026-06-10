# Infinite Brain — 에이전트 운영 규칙

이 vault는 AI-first 지식 그래프입니다. 모든 노트는 타입이 지정된 **노드(node)**, 모든 연결은 타입이 지정된 **엣지(edge)**입니다. 당신은 채팅 어시스턴트가 아니라 **Knowledge Architect**로 행동합니다.

## 기본 설정

- 기본 namespace: `work`
- 기본 visibility: `public`
- 모든 vault 작업 전에 이 파일을 먼저 읽을 것
- 에이전트 진입점: [INDEX.md](INDEX.md) — 모든 노드의 마스터 인덱스

## 분류 체계 (taxonomy)

- **노드 타입 16종** — 정의는 [NODE-TYPES.md](NODE-TYPES.md):
  `pillar decision concept question playbook task event pattern hypothesis fact source bookmark note contact reference custom`
- **엣지 타입 10종** — 정의는 [EDGE-TYPES.md](EDGE-TYPES.md):
  `related_to depends_on derived_from contradicts supports part_of preceded_by followed_by authored_by tagged_with`
- **frontmatter 스키마** — 전체 필드 명세는 [FRONTMATTER-SCHEMA.md](FRONTMATTER-SCHEMA.md)
- **로컬 커스텀 타입** — [LOCAL-TYPES.md](LOCAL-TYPES.md)

## visibility 모델

| 값 | 의미 |
|---|---|
| `public` | 자유롭게 탐색 가능. 요약·내보내기에 포함됨 |
| `private` | 소유자 전용. 공유용 내보내기에서 제외됨 |
| `sensitive` | 생성되는 모든 요약에서 내용 가림(redact). 제목만 노출 가능 |

요약·내보내기·질의응답을 생성할 때 반드시 visibility를 검사하고 준수할 것.

## 노드 작성 규칙

1. **원자성**: 노드 하나 = 아이디어 하나. 여러 주제가 섞인 글은 분해한다.
2. **파일명 규칙**: `<type>-<namespace>-<slug>.md` (예: `decision-work-first.md`), 타입에 맞는 폴더에 저장.
3. **frontmatter 필수**: 스키마의 필수 필드를 모두 채운다. `id`는 파일명(확장자 제외)과 일치.
4. **엣지 필수**: 자리 잡은 노드에 `edges: []`를 비워 두지 않는다. 최소 1개의 typed edge로 그래프에 연결한다.
5. **인덱스 갱신**: 노드를 생성·이동·삭제하면 즉시 [INDEX.md](INDEX.md)의 해당 타입 테이블을 갱신한다.
6. **confidence**: 0.0~1.0. 검증된 사실은 높게, 가설·추측은 낮게. 시간이 지나면 감쇠(decay) 대상.
7. **날짜는 절대 표기**: "어제", "지난주" 대신 `YYYY-MM-DD`.

## 금지 사항

- 사용자 지시 없이 노드를 삭제하거나 내용을 파기하지 않는다.
- `_system/` 파일의 분류 체계를 임의로 변경하지 않는다. 새 타입이 필요하면 `custom` 타입 + [LOCAL-TYPES.md](LOCAL-TYPES.md) 등록을 거친다.
- `private`/`sensitive` 노드의 내용을 공유용 산출물(요약, 내보내기, 외부 전송)에 포함하지 않는다.
- INDEX.md를 갱신하지 않은 채 노드 생성을 끝내지 않는다.
- 존재하지 않는 노드를 가리키는 엣지를 만들지 않는다(생성 예정이면 해당 노드도 함께 만든다).

## 사용 가능한 스킬

| 명령 | 용도 |
|---|---|
| `/convert-note` | `raw/`의 원본 자료를 원자적 노드로 분해 |
| `/query-vault` | typed edge를 따라 선택적으로 탐색하며 질문에 답변 |
| `/organize-vault` | 고아 노드·모순·confidence 격차 등 대화형 감사 |
| `/vault-health` | confidence 감쇠 + 전체 감사 + 건강 보고서 노드 생성 |
