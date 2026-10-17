# EtherChannel 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: 2026-10-17
- Aruba CX 교체: BR1 스위치를 Aruba CX(ArubaOS-CX Virtual 10.07)로 교체하고 AOS-CX 명령 기준으로 재검증

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-SW1-L3 | `show lacp aggregates` | LAG 1(1/1/3, 1/1/4) LACP active | lag1, Interfaces 1/1/3 1/1/4, Heartbeat rate Slow, Hash l3-src-dst, Aggregate mode Active | 통과 |
| BR1-SW2-L3 | `show lacp aggregates` | LAG 1(1/1/3, 1/1/4) LACP active | lag1, Interfaces 1/1/4 1/1/3, Heartbeat rate Slow, Hash l3-src-dst, Aggregate mode Active | 통과 |

BR1-SW1-L3 Po1
![BR1-SW1-L3 Po1](images/Pasted%20image%2020261017185348.png)

BR1-SW2-L3 Po1
![BR1-SW2-L3 Po1](images/Pasted%20image%2020261017185336.png)