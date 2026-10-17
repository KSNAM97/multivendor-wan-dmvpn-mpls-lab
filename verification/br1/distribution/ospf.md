# OSPF 검증 (BR1 / distribution)

- Jira: KAN-14
- 대상 장비: BR1-SW1-L3(12.12.12.11), BR1-SW2-L3(12.12.12.12)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 2~4절
- 검증일: 2026-10-12
- Aruba CX 교체: BR1 스위치를 Aruba CX로 교체해 명령은 AOS-CX 기준이며, 이전 결과는 Cisco 구성 기록이라 교체 후 재확인 필요

| 장비                                               | 명령                             | 기대 결과                                                | 실제 결과 | 판정  |
| ------------------------------------------------ | ------------------------------ | ---------------------------------------------------- | ----- | --- |
| BR1-SW1-L3(12.12.12.11), BR1-SW2-L3(12.12.12.12) | `show ip ospf neighbors` | 모든 이웃 FULL | (Cisco 구성 기록) BR1-SW1-L3, SW2-L3 각 2개(BR1-R1, BR1-R2) 모두 FULL, 스위치 간 이웃 없음(Po1은 L2) | 재확인 |
| BR1-SW1-L3(12.12.12.11), BR1-SW2-L3(12.12.12.12) | `show ip route ospf` | 다른 장비 `lo0`와 반대편 서브넷 학습 | (Cisco 구성 기록) 내부 /30 링크, 라우터·상대 스위치 lo0(12.12.12.1, .2, 상대 스위치) 학습, 13개 서브넷. 타 사이트 경로 없음 | 재확인 |
| BR1-SW1-L3(12.12.12.11), BR1-SW2-L3(12.12.12.12) | `show ip ospf interface brief` | 사용자 VLAN SVI와 `lo0`는 passive, 라우티드 포트 point-to-point | (Cisco 구성 기록) Lo0 LOOP, Vl10, Vl20 DR Nbrs 0/0(passive), Gi0/0, Gi0/1 P2P 1/1, 전체 Area 2 | 재확인 |

프로세스 12, Area 2.

BR1-SW1-L3 OSPF
![BR1-SW1-L3 OSPF](images/Pasted%20image%2020261012203146.png)

BR1-SW2-L3 OSPF
![BR1-SW2-L3 OSPF](images/Pasted%20image%2020261012203158.png)