# EtherChannel 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: 2026-10-13

| 장비         | 명령                          | 기대 결과                            | 실제 결과 | 판정  |
| ---------- | --------------------------- | -------------------------------- | ----- | --- |
| BR1-SW1-L3 | `show etherchannel summary` | Po1(Gi1/0, Gi1/1) LACP, 멤버 `(P)` | Po1(SU) LACP, Gi1/0(P), Gi1/1(P) | 통과 |
| BR1-SW2-L3 | `show etherchannel summary` | Po1(Gi1/0, Gi1/1) LACP, 멤버 `(P)` | Po1(SU) LACP, Gi1/0(P), Gi1/1(P) | 통과 |
BR1-SW1-L3 Po1
![BR1-SW1-L3 Po1](images/Pasted%20image%2020261013181037.png)

BR1-SW2-L3 Po1
![BR1-SW2-L3 Po1](images/Pasted%20image%2020261013181054.png)