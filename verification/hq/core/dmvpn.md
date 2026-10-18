# DMVPN Spoke 검증 (HQ / core)

- Jira: KAN-33
- 대상 장비: HQ-R2
- 기준 문서: [dmvpn-design.md](../../../docs/dmvpn-design.md) 1~4절
- 검증일: 2026-10-18

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| HQ-R2 | `show dmvpn` | Hub(172.16.0.1) Up, Tunnel0 172.16.0.13, NBMA 203.0.113.9 | Tunnel0 Type:Spoke, NHRP Peers 1. Hub 203.0.113.5(172.16.0.1) State UP, Attrb S(Static) | 통과 |
| HQ-R2 | `show ip ospf neighbor` | Tunnel0에서 DC-R2와 FULL(Spoke priority 0) | Tunnel0에서 10.10.10.2(172.16.0.1) FULL/DR(Pri 255). Gi4/0에서 Private-net 203.0.113.10(Private-net 2.2.2.2) FULL. 사이트 내부 이웃 11.11.11.1(10.1.0.17, Gi2/0), 11.11.11.13(10.1.0.14, Gi1/0), 11.11.11.12(10.1.0.6, Gi0/0) | 통과 |
| HQ-R2 | `show ip ospf interface tunnel 0` | Spoke priority 0, Hub(DR)와 인접 | Tunnel0 up/up, 172.16.0.13/24, Area 0, 프로세스 11, Router ID 11.11.11.2, BROADCAST, State DROTHER, Priority 0, DR는 10.10.10.2(172.16.0.1), BDR 없음. Neighbor Count 1, Adjacent neighbor 1(10.10.10.2, DR) | 통과 |
| HQ-R2 | `show crypto isakmp sa` | Hub와 QM_IDLE | dst 203.0.113.5, src 203.0.113.9 QM_IDLE ACTIVE(conn-id 1002). 이전 시도의 MM_NO_STATE(deleted) 항목이 함께 표시됨 | 통과 |
| HQ-R2 | `show ip nhrp` | Hub(172.16.0.1) static 항목, NBMA 203.0.113.5 | 172.16.0.1/32 via 172.16.0.1, Type static, Flags used, NBMA 203.0.113.5, never expire | 통과 |
| HQ-R2 | `show crypto ipsec sa` | Hub와 SA, 암호화 카운트 증가 | Tunnel0, profile dmvpn-profile-head-1, local 203.0.113.9, 상대 203.0.113.5(GRE/47). encaps 25, decaps 23, recv errors 0, send errors 86(SA 수립 전 시도분), PFS Y, DH group5 | 통과 |
| HQ-R2 | `ping 다른 Spoke 사이트 호스트(2회 이상)` | Phase 3 shortcut 생성, `show ip nhrp`에 Spoke 간 dynamic 항목, `traceroute`가 Hub를 거치지 않음 | - | 미확인 |

Phase 3 shortcut 검증은 2주 금(10/23)에 수행.

HQ-R2 show ip ospf interface tunnel 0
![HQ-R2 OSPF interface tunnel 0](images/dmvpn-hq-r2-ospf-interface.webp)

HQ-R2 dmvpn
![HQ-R2 dmvpn](images/dmvpn-hq-r2-dmvpn.webp)

HQ-R2 nhrp
![HQ-R2 nhrp](images/dmvpn-hq-r2-nhrp.png)

HQ-R2 isakmp
![HQ-R2 isakmp](images/dmvpn-hq-r2-isakmp.webp)

HQ-R2 ipsec
![HQ-R2 ipsec](images/dmvpn-hq-r2-ipsec.webp)

HQ-R2 ospf-neighbor
![HQ-R2 ospf-neighbor](images/dmvpn-hq-r2-ospf-neighbor.webp)
