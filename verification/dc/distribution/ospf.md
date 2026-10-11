# OSPF 검증 (DC / distribution)

- Jira: KAN-14
- 대상 장비: DC-SW1-L3~SW4-L3(10.10.10.11~14)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 2~4절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-SW1-L3~SW4-L3(10.10.10.11~14) | `show ip ospf neighbor` | 모든 이웃 FULL | - | 미확인 |
| DC-SW1-L3~SW4-L3(10.10.10.11~14) | `show ip route ospf` | 다른 장비 `lo0`와 반대편 서브넷 학습 | - | 미확인 |
| DC-SW1-L3~SW4-L3(10.10.10.11~14) | `show ip ospf interface brief` | 사용자 VLAN SVI와 `lo0`는 passive, 라우티드 포트 point-to-point | - | 미확인 |

프로세스 10, Area 0.
