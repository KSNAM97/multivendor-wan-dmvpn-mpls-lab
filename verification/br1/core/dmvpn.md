# DMVPN Spoke 검증 (BR1 / core)

- Jira: KAN-22
- 대상 장비: BR1-R1
- 기준 문서: [dmvpn-design.md](../../../docs/dmvpn-design.md) 1~4절
- 검증일: 2026-10-18

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-R1 | `show dmvpn` | Hub(172.16.0.1) Up, Tunnel0 172.16.0.11, NBMA 203.0.113.13 | - | 미확인 |
| BR1-R1 | `show ip ospf neighbor` | Tunnel0에서 DC-R2와 FULL(Spoke priority 0) | - | 미확인 |
| BR1-R1 | `show ip ospf interface tunnel 0` | Spoke priority 0, Hub(DR)와 인접 | Tunnel0 up/up, 172.16.0.11/24, Area 0, 프로세스 12, Router ID 12.12.12.1, BROADCAST, State DROTHER, Priority 0, DR는 10.10.10.2(172.16.0.1), BDR 없음. Neighbor Count 1, Adjacent neighbor 1(10.10.10.2, DR) | 통과 |
| BR1-R1 | `show crypto isakmp sa` | Hub와 QM_IDLE | - | 미확인 |
| BR1-R1 | `ping 다른 Spoke 사이트 호스트(2회 이상)` | Phase 3 shortcut 생성, `show ip nhrp`에 Spoke 간 dynamic 항목, `traceroute`가 Hub를 거치지 않음 | - | 미확인 |

Phase 3 shortcut 검증은 2주 금(10/23)에 수행.

BR1-R1 show ip ospf interface tunnel 0
![BR1-R1 OSPF interface tunnel 0](images/dmvpn-br1-r1-ospf-interface.webp)
