# OSPF 검증 (BR2 / core)

- Jira: KAN-14
- 대상 장비: BR2-R1(13.13.13.1), BR2-R2(13.13.13.2, ABR)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 2~4절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-R1(13.13.13.1), BR2-R2(13.13.13.2, ABR) | `show ip ospf neighbor` | 모든 이웃 FULL, 네트워크 타입 point-to-point | - | 미확인 |
| BR2-R1(13.13.13.1), BR2-R2(13.13.13.2, ABR) | `show ip route ospf` | 사이트 /30 링크, 사용자·서버 VLAN, 다른 장비 `lo0` 학습(프로세스 13) | - | 미확인 |

프로세스 13, Area 3, Router ID는 장비의 `lo0`.
