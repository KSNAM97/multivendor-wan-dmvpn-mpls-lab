# VLAN, 트렁크 검증 (BR2 / distribution)

- Jira: KAN-13
- 대상 장비: BR2-SW1-L3, BR2-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-SW1-L3 | `show interfaces trunk` | Po1과 BR2-SW3-L2 방향 `Gi2/0` trunking, native 999, 허용 10,100 | - | 미확인 |
| BR2-SW2-L3 | `show interfaces trunk` | Po1과 BR2-SW3-L2 방향 `Gi2/0` trunking, native 999, 허용 10,100 | - | 미확인 |
| BR2-SW1-L3, BR2-SW2-L3 | `show vlan brief` | VLAN 10 br2-user, 100 mgmt, 999 native active | - | 미확인 |

트렁크 링크: BR2-SW1-L3 Gi2/0 ↔ BR2-SW3-L2 Gi0/0, BR2-SW2-L3 Gi2/0 ↔ BR2-SW3-L2 Gi0/1
