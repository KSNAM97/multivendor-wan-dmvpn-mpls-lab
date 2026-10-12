# VLAN, 트렁크 검증 (BR2 / access)

- Jira: KAN-13
- 대상 장비: BR2-SW3-L2
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-SW3-L2 | `show interfaces trunk` | `Gi0/0`, `Gi0/1` trunking, native 999, 허용 10,20,100 | - | 미확인 |
| BR2-SW3-L2 | `show vlan brief` | VLAN 10 br2-public(Gi3/0, BR2-PC1), VLAN 20 br2-private(Gi3/1, BR2-PC2) | - | 미확인 |
