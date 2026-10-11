# STP(MST) 검증 (BR2 / access)

- Jira: KAN-13
- 대상 장비: BR2-SW3-L2
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 3절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-SW3-L2 | `show spanning-tree mst 1` | 기본 priority, 루트는 SW1, 호스트 포트 Desg FWD Edge | - | 미확인 |
