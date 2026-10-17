# STP(MST) 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 3절
- 검증일: 2026-10-17
- Aruba CX 교체: BR1 스위치를 Aruba CX(ArubaOS-CX Virtual 10.07)로 교체하고 AOS-CX 명령 기준으로 재검증

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-SW1-L3 | `show spanning-tree mst` | 루트(priority 4096), region br1, instance 1 VLAN 10,20,100 | MST1 VLAN 10,20,100, Bridge와 Root 모두 08:00:09:48:bf:8f priority 4096(이 스위치가 루트), 1/1/5와 lag1 Designated FWD. region 이름은 이 출력에 없음(`show spanning-tree mst-config`에서 SW3 br1 확인) | 통과 |
| BR1-SW2-L3 | `show spanning-tree mst` | 보조 루트(8192), 루트는 SW1 | MST1 Bridge priority 8192, Root 08:00:09:48:bf:8f(BR1-SW1-L3, 4096), Root 포트 lag1(cost 20000) FWD, 1/1/5 Designated FWD(MST0에서는 Alternate BLK) | 통과 |

BR1-SW1-L3 MSTP
![BR1-SW1-L3 MSTP](images/Pasted%20image%2020261017185408.png)


BR1-SW2-L3 MSTP
![BR1-SW2-L3 MSTP](images/Pasted%20image%2020261017185420.png)