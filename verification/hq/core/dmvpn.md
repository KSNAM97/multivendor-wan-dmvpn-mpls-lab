# DMVPN Spoke 검증 (HQ / core)

- Jira: KAN-33
- 대상 장비: HQ-R2
- 기준 문서: [dmvpn-design.md](../../../docs/dmvpn-design.md) 1~4절
- 검증일: 2026-10-18

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| HQ-R2 | `show dmvpn` | Hub(172.16.0.1) Up, Tunnel0 172.16.0.13, NBMA 203.0.113.9 | - | 미확인 |
| HQ-R2 | `show ip ospf neighbor` | Tunnel0에서 DC-R2와 FULL(Spoke priority 0) | - | 미확인 |
| HQ-R2 | `show ip ospf interface tunnel 0` | Spoke priority 0, Hub(DR)와 인접 | Tunnel0 up/up, 172.16.0.13/24, Area 0, 프로세스 11, Router ID 11.11.11.2, BROADCAST, State DROTHER, Priority 0, DR는 10.10.10.2(172.16.0.1), BDR 없음. Neighbor Count 1, Adjacent neighbor 1(10.10.10.2, DR) | 통과 |
| HQ-R2 | `show crypto isakmp sa` | Hub와 QM_IDLE | - | 미확인 |
| HQ-R2 | `ping 다른 Spoke 사이트 호스트(2회 이상)` | Phase 3 shortcut 생성, `show ip nhrp`에 Spoke 간 dynamic 항목, `traceroute`가 Hub를 거치지 않음 | - | 미확인 |

Phase 3 shortcut 검증은 2주 금(10/23)에 수행.

HQ-R2 show ip ospf interface tunnel 0
![HQ-R2 OSPF interface tunnel 0](images/dmvpn-hq-r2-ospf-interface.webp)
