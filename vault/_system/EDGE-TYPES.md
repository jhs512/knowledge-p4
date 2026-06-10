# 엣지 타입 정의 (10종)

엣지는 노드 frontmatter의 `edges` 배열에 기록합니다. 방향은 항상 **이 노드 → target**입니다.

```yaml
edges:
  - type: supports
    target: pillar-work-foundation
```

| 타입 | 의미 (이 노드 → target) | 사용 시점 |
|---|---|---|
| `related_to` | 단순 연관 | 더 구체적인 타입이 없을 때 최후의 수단 |
| `depends_on` | target이 선행되어야 함 | task 간 의존, 개념의 전제 |
| `derived_from` | target에서 파생·도출됨 | fact ← source, fact ← hypothesis 검증 |
| `contradicts` | target과 모순됨 | 상충하는 fact/decision 발견 시. 방치 금지 — organize-vault가 감사 |
| `supports` | target을 뒷받침·강화함 | decision → pillar, fact → hypothesis |
| `part_of` | target의 구성 요소임 | concept → 더 큰 concept, task → playbook |
| `preceded_by` | target이 시간상 먼저임 | event 체인, decision 이력 |
| `followed_by` | target이 시간상 다음임 | event 체인 (preceded_by의 역방향) |
| `authored_by` | target(contact)이 작성·발화함 | source → contact |
| `tagged_with` | target(개념·주제)으로 분류됨 | 폭넓은 주제 묶기 |

## 규칙

- 자리 잡은 노드는 `edges: []`를 비워 두지 않는다 (고아 노드 금지).
- `related_to` 남용 금지 — 더 구체적인 타입을 먼저 검토한다.
- `contradicts` 엣지는 발견 즉시 기록하고, 해소되면 한쪽의 confidence를 조정하거나 노드를 대체한다.
- target은 노드 `id`(파일명에서 `.md` 제외)로 적는다.
