# MPLS 검증 (ISP2 / core)

- Jira: KAN-34(LDP), KAN-35(VRF, vpnv4)
- 대상 장비: ISP-2-PE-A, P1, P2, PE-B
- 기준 문서: [mpls-design.md](../../../docs/mpls-design.md) 2~4절, 6절
- 검증일: -

## 1. LDP (KAN-34)

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-2-PE-A, PE-B | `show mpls ldp neighbor` | P1, P2와 LDP 이웃 2개 Oper | - | 미확인 |
| ISP-2-P1, P2 | `show mpls ldp neighbor` | PE-A, PE-B, 상대 P와 LDP 이웃 3개 Oper | - | 미확인 |
| ISP-2-PE-A, PE-B, P1, P2 | `show mpls interfaces` | 내부 링크만 LDP 활성(PE의 사이트 방향 포트는 제외) | - | 미확인 |
| ISP-2-PE-A, PE-B, P1, P2 | `show mpls forwarding-table` | 상대 장비 Loopback0 `/32` 3개에 레이블 | - | 미확인 |

## 2. VRF와 vpnv4 (KAN-35)

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-2-PE-A, PE-B | `show ip vrf` | `corp`, RD 65200:1 | - | 미확인 |
| ISP-2-PE-A, PE-B | `show ip vrf interfaces` | 사이트 방향 포트 Gi2/0, Gi3/0이 `corp` 소속 | - | 미확인 |
| ISP-2-P1, P2 | `show ip bgp vpnv4 all summary` | PE-A, PE-B, 상대 RR과 vpnv4 Established(3세션) | - | 미확인 |
| ISP-2-PE-A, PE-B | `show ip bgp vpnv4 all summary` | P1, P2와 vpnv4 Established(2세션) | - | 미확인 |

## 3. 사이트 경로와 레이블 경로 (사이트 eBGP 이후)

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-2-PE-A, PE-B | `show ip route vrf corp` | 사이트 /16(CE에서 eBGP, 상대 PE에서 vpnv4) | - | 미확인 |
| 사이트 호스트 | `traceroute <다른 사이트 호스트>` | ISP 구간에 `[MPLS: Label ...]` 표시 | - | 미확인 |
