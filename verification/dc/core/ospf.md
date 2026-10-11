# OSPF 검증 (DC / core)

- Jira: KAN-14
- 대상 장비: DC-R1~R4(10.10.10.1~4)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 2~4절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-R1~R4(10.10.10.1~4) | `show ip ospf neighbor` | 모든 이웃 FULL, 네트워크 타입 point-to-point | - | 미확인 |
| DC-R1~R4(10.10.10.1~4) | `show ip route ospf` | 사이트 /30 링크, 사용자·서버 VLAN, 다른 장비 `lo0` 학습(프로세스 10) | - | 미확인 |

프로세스 10, Area 0, Router ID는 장비의 `lo0`.
