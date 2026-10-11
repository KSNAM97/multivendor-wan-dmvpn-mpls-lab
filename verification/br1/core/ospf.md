# OSPF 검증 (BR1 / core)

- Jira: KAN-14
- 대상 장비: BR1-R1(12.12.12.1, ABR), BR1-R2(12.12.12.2)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 2~4절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-R1(12.12.12.1, ABR), BR1-R2(12.12.12.2) | `show ip ospf neighbor` | 모든 이웃 FULL, 네트워크 타입 point-to-point | - | 미확인 |
| BR1-R1(12.12.12.1, ABR), BR1-R2(12.12.12.2) | `show ip route ospf` | 사이트 /30 링크, 사용자·서버 VLAN, 다른 장비 `lo0` 학습(프로세스 12) | - | 미확인 |

프로세스 12, Area 2, Router ID는 장비의 `lo0`.
