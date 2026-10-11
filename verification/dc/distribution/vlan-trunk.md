# VLAN, 트렁크 검증 (DC / distribution)

- Jira: KAN-10
- 대상 장비: DC-SW3-L3, DC-SW4-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 5절
- 검증일: 2026-10-11

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-SW3-L3 | `show running-config \| grep -E "native vlan\|allowed vlan"` | Po1, Et7, Et8에 native 999, 허용 50,100, mode trunk 각 3줄 | native 999 3줄, allowed 50,100 3줄, mode trunk 3줄, `no switchport` 4줄(Et1~Et4), `access vlan` 없음 | 통과 |
| DC-SW3-L3, DC-SW4-L3 | `show interfaces trunk` | Et7, Et8, Po1 trunking, 허용 50,100 | SW3, SW4 모두 Et7, Et8, Po1 trunking, native 999, 허용·active·STP forwarding 50,100 | 통과 |

DC-SW3-L3
![DC-SW3-L3](images/Pasted%20image%2020261011234312.png)

DC-SW4-L3
![DC-SW4-L3](images/Pasted%20image%2020261011234333.png)