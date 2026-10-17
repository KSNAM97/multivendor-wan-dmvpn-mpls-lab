# STP(MST) 검증 (BR1 / access)

- Jira: KAN-12
- 대상 장비: BR1-SW3-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 3절
- 검증일: 2026-10-13
- Aruba CX 교체: BR1 스위치를 Aruba CX로 교체해 명령은 AOS-CX 기준이며, 이전 결과는 Cisco 구성 기록이라 교체 후 재확인 필요

| 장비         | 명령                         | 기대 결과                                      | 실제 결과                                                                                                                                                                               | 판정  |
| ---------- | -------------------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| BR1-SW3-L3 | `show spanning-tree mst` | 기본 priority, 루트는 SW1, 호스트 포트 Desg FWD Edge | (Cisco 구성 기록) 루트 브리지 주소 0c0a.f657(BR1-SW1-L3), 자기 priority 32769(기본), Gi0/0 Root FWD, Gi0/1 Altn BLK, Gi3/0과 Gi3/1 Desg FWD P2p Edge | 재확인 |

BR1-SW3-L3 MSTP
![BR1-SW3-L3](images/Pasted%20image%2020261013184255.png)