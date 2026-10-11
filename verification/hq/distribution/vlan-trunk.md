# VLAN, 트렁크 검증 (HQ / distribution)

- Jira: KAN-8
- 대상 장비: HQ-SW-2, HQ-SW-3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 5절
- 검증일: 2026-10-11

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| HQ-SW-2 | `show interfaces trunk` | Po1, Po3 trunking, native 999, 허용 10,20,100 | Po1, Po3 802.1q trunking, native 999, 허용·active 10,20,100 | 통과 |
| HQ-SW-3 | `show interfaces trunk` | Po2, Po3 trunking, native 999, 허용 10,20,100 | Po2, Po3 802.1q trunking, native 999, 허용·active·STP forwarding 10,20,100 | 통과 |
| HQ-SW-2 | `show vlan brief` | VLAN 10 hq-public, 20 hq-private, 100 mgmt, 999 native active | 4개 VLAN 모두 active, 이름 일치 | 통과 |
| HQ-SW-3 | `show vlan brief` | VLAN 10 hq-public, 20 hq-private, 100 mgmt, 999 native active | 4개 VLAN 모두 active, 이름 일치 | 통과 |

HQ-SW-2 TRUNK
![HQ-SW-2 TRUNK](images/Pasted%20image%2020261011235542.png)

HQ-SW-3 TRUNK
![HQ-SW-3 TRUNK](images/Pasted%20image%2020261011235614.png)

HQ-SW-2 VLAN
![HQ-SW-2 VLAN](images/Pasted%20image%2020261011235550.png)

HQ-SW-3 VLAN
![HQ-SW-3 VLAN](images/Pasted%20image%2020261011235602.png)