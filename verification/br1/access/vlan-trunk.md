# VLAN, 트렁크 검증 (BR1 / access)

- Jira: KAN-12
- 대상 장비: BR1-SW3-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: 2026-10-13
- Aruba CX 교체: BR1 스위치를 Aruba CX로 교체해 명령은 AOS-CX 기준이며, 이전 결과는 Cisco 구성 기록이라 교체 후 재확인 필요

| 장비         | 명령                      | 기대 결과                                                                   | 실제 결과                                                                                                                                           | 판정  |
| ---------- | ----------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| BR1-SW3-L3 | `show vlan port <포트>` | `1/1/1`, `1/1/2` trunking, native 999, 허용 10,20,100 | (Cisco 구성 기록) Gi0/0, Gi0/1 trunking, native 999, 허용 10,20,100 | 재확인 |
| BR1-SW3-L3 | `show vlan` | VLAN 10 br1-public(1/1/6, BR1-PC1), VLAN 20 br1-private(1/1/7, BR1-PC2) | (Cisco 구성 기록) VLAN 10 br1-public(Gi3/0), VLAN 20 br1-private(Gi3/1), 100 mgmt, 999 native active | 재확인 |

BR1-SW3-L3 TRUNK
![BR1-SW3-L3](images/Pasted%20image%2020261013184306.png)

BR1-SW3-L3 VLAN
![BR1-SW3-L3](images/Pasted%20image%2020261013184320.png)