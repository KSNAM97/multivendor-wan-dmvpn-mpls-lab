# VLAN, 트렁크 검증 (BR1 / access)

- Jira: KAN-12
- 대상 장비: BR1-SW3-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: 2026-10-13

| 장비         | 명령                      | 기대 결과                                                                   | 실제 결과                                                                                                                                           | 판정  |
| ---------- | ----------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| BR1-SW3-L3 | `show interfaces trunk` | `Gi0/0`, `Gi0/1` trunking, native 999, 허용 10,20,100 | Gi0/0, Gi0/1 trunking, native 999, 허용 10,20,100 | 통과 |
| BR1-SW3-L3 | `show vlan brief` | VLAN 10 br1-public(Gi3/0, BR1-PC1), VLAN 20 br1-private(Gi3/1, BR1-PC2) | VLAN 10 br1-public(Gi3/0), VLAN 20 br1-private(Gi3/1), 100 mgmt, 999 native active | 통과 |

BR1-SW3-L3 TRUNK
![BR1-SW3-L3](images/Pasted%20image%2020261013184306.png)

BR1-SW3-L3 VLAN
![BR1-SW3-L3](images/Pasted%20image%2020261013184320.png)