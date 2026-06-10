# 노드 타입 정의 (16종)

각 노드는 아래 타입 중 하나를 가지며, 타입과 같은 이름의 복수형 폴더에 저장됩니다.

| 타입 | 폴더 | 정의 | 예시 |
|---|---|---|---|
| `pillar` | `pillars/` | 삶·일의 최상위 영역. 그래프의 뿌리이며 다른 노드들이 모이는 기둥 | "백엔드 역량", "건강" |
| `decision` | `decisions/` | 내린(또는 내릴) 결정과 그 근거. 번복 시 새 decision으로 대체 | "DB는 PostgreSQL로 한다" |
| `concept` | `concepts/` | 재사용 가능한 개념·정의·멘탈 모델 | "이벤트 소싱", "CAP 정리" |
| `question` | `questions/` | 아직 답이 없는 열린 질문. 답이 생기면 fact/decision으로 파생 | "캐시 무효화 전략은?" |
| `playbook` | `playbooks/` | 반복 수행하는 절차·체크리스트·SOP | "배포 절차", "주간 회고" |
| `task` | `tasks/` | 수행할 작업 항목. status로 진행 상태 추적 | "인덱스 추가하기" |
| `event` | `events/` | 특정 시점에 일어난 일의 기록 | "2026-06-10 장애 발생" |
| `pattern` | `patterns/` | 반복 관찰되는 패턴·경향 | "월요일에 트래픽 급증" |
| `hypothesis` | `hypotheses/` | 검증 전 가설. confidence 낮게 시작, 검증되면 fact로 파생 | "N+1이 지연 원인일 것" |
| `fact` | `facts/` | 검증된 사실. source 엣지로 출처 연결 권장 | "서비스 P95는 320ms" |
| `source` | `sources/` | 정보의 출처(책, 논문, 영상, 대화). fact/concept가 derived_from으로 참조 | "DDIA 11장" |
| `bookmark` | `bookmarks/` | 나중에 볼 URL·자료 포인터. 소화되면 다른 타입으로 승격 | "Rust async 블로그 글" |
| `note` | `notes/` | 아직 분류되지 않은 자유 형식 메모. 임시 거처 | 회의 중 끄적임 |
| `contact` | `contacts/` | 사람·조직 정보 | "팀 동료 김OO" |
| `reference` | `references/` | 자주 찾는 참조 자료·치트시트·스펙 | "HTTP 상태코드 표" |
| `custom` | `custom/` | 위에 없는 타입. [LOCAL-TYPES.md](LOCAL-TYPES.md)에 등록 후 사용 | — |

## 승격 흐름 (권장)

- `raw/` 원본 → `/convert-note` → 원자적 노드들
- `note` → 내용이 분명해지면 적절한 타입으로 이동
- `bookmark` → 읽고 소화하면 `source` + `fact`/`concept`
- `hypothesis` → 검증되면 `fact` (`derived_from` 엣지로 연결)
- `question` → 답이 나오면 `fact` 또는 `decision`
