# OSPF 검증 (DC / core)

- Jira: KAN-14
- 대상 장비: DC-R1~R4(10.10.10.1~4)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 2~4절
- 검증일: 2026-10-12

| 장비                     | 명령                      | 기대 결과                                            | 실제 결과 | 판정  |
| ---------------------- | ----------------------- | ------------------------------------------------ | ----- | --- |
| DC-R1~R4(10.10.10.1~4) | `show ip ospf neighbor` | 모든 이웃 FULL, 네트워크 타입 point-to-point | DC-R1, R2는 DC-SW1-L3, DC-SW2-L3와 2개, DC-R3, R4는 DC-SW3-L3, DC-SW4-L3, 상대 라우터와 3개, 모두 FULL(P2P) | 통과 |
| DC-R1~R4(10.10.10.1~4) | `show ip route ospf` | 사이트 /30 링크, 사용자·서버 VLAN, 다른 장비 `lo0` 학습(프로세스 10) | 23개 서브넷 학습: 사이트 /30 링크, 서버 VLAN 10.2.50.0/24, 다른 장비 lo0(10.10.10.1~4, 11~14) | 통과 |

프로세스 10, Area 0, Router ID는 장비의 `lo0`.
DC-R1 OSPF
![DC-R1 OSPF](images/Pasted%20image%2020261012191727.png)

DC-R2 OSPF
![DC-R2 OSPF](images/Pasted%20image%2020261012191753.png)

DC-R3 OSPF
![DC-R3 OSPF](images/Pasted%20image%2020261012191821.png)

DC-R4 OSPF
![DC-R4 OSPF](images/Pasted%20image%2020261012191853.png)

