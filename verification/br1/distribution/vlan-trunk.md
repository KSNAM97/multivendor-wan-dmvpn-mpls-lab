# VLAN, 트렁크 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-SW1-L3 | `show interfaces trunk` | Po1과 BR1-SW3-L3 방향 `Gi2/0` trunking, native 999, 허용 10,100 | - | 미확인 |
| BR1-SW2-L3 | `show interfaces trunk` | Po1과 BR1-SW3-L3 방향 `Gi2/0` trunking, native 999, 허용 10,100 | - | 미확인 |
| BR1-SW1-L3, BR1-SW2-L3 | `show vlan brief` | VLAN 10 br1-user, 100 mgmt, 999 native active | - | 미확인 |

트렁크 링크: BR1-SW1-L3 Gi2/0 ↔ BR1-SW3-L3 Gi0/0, BR1-SW2-L3 Gi2/0 ↔ BR1-SW3-L3 Gi0/1
