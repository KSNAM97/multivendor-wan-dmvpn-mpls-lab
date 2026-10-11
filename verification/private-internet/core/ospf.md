# Private Internet OSPF 검증

- Jira: KAN-20
- 대상 장비: Private-net, HQ-R2, DC-R1, DC-R2, BR1-R1, BR2-R2
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 5절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| Private-net | `show ip ospf neighbor` | 프로세스 2, Area 100, 5개 링크 이웃 FULL, Router ID 2.2.2.2 | - | 미확인 |
| 사이트 라우터 | `show ip ospf neighbor` | Private-net과 FULL(프로세스 2) | - | 미확인 |
| 사이트 라우터 | `ping <상대 NBMA>` | DMVPN NBMA 주소(203.0.113.x) 간 도달 | - | 미확인 |
