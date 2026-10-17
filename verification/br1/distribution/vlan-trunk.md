# VLAN, 트렁크 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: 2026-10-13
- Aruba CX 교체: BR1 스위치를 Aruba CX로 교체해 명령은 AOS-CX 기준이며, 이전 결과는 Cisco 구성 기록이라 교체 후 재확인 필요

| 장비                     | 명령                      | 기대 결과                                                           | 실제 결과 | 판정  |
| ---------------------- | ----------------------- | --------------------------------------------------------------- | ----- | --- |
| BR1-SW1-L3 | `show vlan port <포트>` | Po1과 BR1-SW3-L3 방향 `1/1/5` trunking, native 999, 허용 10,20,100 | (Cisco 구성 기록) Po1, Gi2/0 trunking, native 999, 허용 10,20,100, 전부 STP forwarding | 재확인 |
| BR1-SW2-L3 | `show vlan port <포트>` | Po1과 BR1-SW3-L3 방향 `1/1/5` trunking, native 999, 허용 10,20,100 | (Cisco 구성 기록) Po1, Gi2/0 trunking, native 999, 허용 10,20,100, 전부 STP forwarding | 재확인 |
| BR1-SW1-L3, BR1-SW2-L3 | `show vlan` | VLAN 10 br1-public, 20 br1-private, 100 mgmt, 999 native active | (Cisco 구성 기록) VLAN 10 br1-public, 20 br1-private, 100 mgmt, 999 native 모두 active (BR1-SW1-L3, SW2-L3 동일) | 재확인 |

트렁크 링크: BR1-SW1-L3 Gi2/0 ↔ BR1-SW3-L3 Gi0/0, BR1-SW2-L3 Gi2/0 ↔ BR1-SW3-L3 Gi0/1
BR1-SW1-L3 TRUNK
![BR1-SW1-L3 TRUNK](images/Pasted%20image%2020261013181305.png)

BR1-SW1-L3 VLAN
![BR1-SW1-L3 VLAN](images/Pasted%20image%2020261013181329.png)

BR1-SW2-L3 TRUNK
![BR1-SW2-L3 TRUNK](images/Pasted%20image%2020261013181352.png)

BR1-SW2-L3 VLAN
![BR1-SW2-L3 VLAN](images/Pasted%20image%2020261013181415.png)