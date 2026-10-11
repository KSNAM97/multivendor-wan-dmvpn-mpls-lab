# ISP 연결 검증 (BR2 / core)

- Jira: KAN-18
- 대상 장비: BR2-R1, BR2-R2
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 6절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-R1, BR2-R2 | `show ip route 0.0.0.0` | 정적 기본 경로 `S*`(ISP 방향), track 연동 | - | 미확인 |
| BR2-R1, BR2-R2 | `show ip sla statistics` | IP SLA 1 성공(5초 주기) | - | 미확인 |
| BR2-R1, BR2-R2 | `show track` | track 1 Up | - | 미확인 |
| BR2-R1, BR2-R2 | `show ip ospf database external` | `default-information originate` 기본 경로 Type 1 광고 | - | 미확인 |

장애 시 정적 경로가 빠지고 기본 경로 광고가 철회되는지는 [scenarios/](../../scenarios/)에서 확인.
