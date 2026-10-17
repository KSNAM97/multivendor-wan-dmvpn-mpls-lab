# EtherChannel 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: 2026-10-13
- Aruba CX 교체: BR1 스위치를 Aruba CX로 교체해 명령은 AOS-CX 기준이며, 이전 결과는 Cisco 구성 기록이라 교체 후 재확인 필요

| 장비         | 명령                          | 기대 결과                            | 실제 결과 | 판정  |
| ---------- | --------------------------- | -------------------------------- | ----- | --- |
| BR1-SW1-L3 | `show lacp aggregates` | LAG 1(1/1/3, 1/1/4) LACP active, 멤버 동기화(`show lacp interfaces`) | (Cisco 구성 기록) Po1(SU) LACP, Gi1/0(P), Gi1/1(P) | 재확인 |
| BR1-SW2-L3 | `show lacp aggregates` | LAG 1(1/1/3, 1/1/4) LACP active, 멤버 동기화(`show lacp interfaces`) | (Cisco 구성 기록) Po1(SU) LACP, Gi1/0(P), Gi1/1(P) | 재확인 |

BR1-SW1-L3 Po1
![BR1-SW1-L3 Po1](images/Pasted%20image%2020261013181037.png)

BR1-SW2-L3 Po1
![BR1-SW2-L3 Po1](images/Pasted%20image%2020261013181054.png)