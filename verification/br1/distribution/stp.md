# STP(MST) 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 3절
- 검증일: 2026-10-13
- Aruba CX 교체: BR1 스위치를 Aruba CX로 교체해 명령은 AOS-CX 기준이며, 이전 결과는 Cisco 구성 기록이라 교체 후 재확인 필요

| 장비         | 명령                         | 기대 결과                                                    | 실제 결과 | 판정  |
| ---------- | -------------------------- | -------------------------------------------------------- | ----- | --- |
| BR1-SW1-L3 | `show spanning-tree mst` | 루트(priority 4096), region br1, instance 1 VLAN 10,20,100 | (Cisco 구성 기록) MST1 VLAN 10,20,100, priority 4097(4096 sysid 1), Root this switch for MST1, Gi2/0과 Po1 Desg FWD. region 이름은 이 출력에 없음(show spanning-tree mst configuration 필요) | 재확인 |
| BR1-SW2-L3 | `show spanning-tree mst` | 보조 루트(8192), 루트는 SW1 | (Cisco 구성 기록) priority 8193(8192 sysid 1), 루트 4097(BR1-SW1-L3), Po1 Root FWD, Gi2/0 Desg FWD | 재확인 |

BR1-SW1-L3 MSTP
![BR1-SW1-L3 MSTP](images/Pasted%20image%2020261013181147.png)


BR1-SW2-L3 MSTP
![BR1-SW2-L3 MSTP](images/Pasted%20image%2020261013181213.png)