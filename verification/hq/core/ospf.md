# OSPF 검증 (HQ / core)

- Jira: KAN-14
- 대상 장비: HQ-R1(11.11.11.1), HQ-R2(11.11.11.2, ABR)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 2~4절
- 검증일: 2026-10-12

| 장비                                        | 명령                      | 기대 결과                                            | 실제 결과 | 판정  |
| ----------------------------------------- | ----------------------- | ------------------------------------------------ | ----- | --- |
| HQ-R1(11.11.11.1), HQ-R2(11.11.11.2, ABR) | `show ip ospf neighbor` | 모든 이웃 FULL, 네트워크 타입 point-to-point | HQ-R1, HQ-R2 각 3개(HQ-SW-2, HQ-SW-3, 상대 라우터) 모두 FULL(P2P) | 통과 |
| HQ-R1(11.11.11.1), HQ-R2(11.11.11.2, ABR) | `show ip route ospf` | 사이트 /30 링크, 사용자·서버 VLAN, 다른 장비 `lo0` 학습(프로세스 11) | 사용자 VLAN 10.1.10.0/24, 10.1.20.0/24, 직접 연결되지 않은 /30 링크, 다른 장비 lo0(11.11.11.12, 11.11.11.13, 상대 라우터) 학습, 7개 서브넷. DMVPN 전이라 타 사이트 경로 없음 | 통과 |

프로세스 11, Area 1, Router ID는 장비의 `lo0`.

HQ-R1 OSPF
![HQ-R1 OSPF](images/Pasted%20image%2020261012193920.png)

![route ospf](images/Pasted%20image%2020261012194309.png)

HQ-R2 OSPF
![HQ-R2 OSPF](images/Pasted%20image%2020261012193941.png)

![route ospf](images/Pasted%20image%2020261012194355.png)