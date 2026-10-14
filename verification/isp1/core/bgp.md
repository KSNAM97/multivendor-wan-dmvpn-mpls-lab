# BGP 검증(iBGP RR, 사이트 eBGP) (ISP1 / core)

- Jira: KAN-15, KAN-16(iBGP), KAN-17(PE 정책), KAN-18(사이트 eBGP)
- 대상 장비: ISP-1-PE-A, P1, P2, PE-B
- 기준 문서: [bgp-design.md](../../../docs/bgp-design.md) 2~6절
- 검증일: -

## 1. ISP 내부 OSPF와 iBGP (KAN-15, 16, 3주 수)

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-1-PE-A, PE-B, P1, P2 | `show ip ospf neighbor` | ISP 내부 이웃 Full(point-to-point), Loopback0 `100.100.100.1~4` 도달 | - | 미확인 |
| ISP-1-P1, P2 | `show ip bgp summary` | AS 65100, PE-A, PE-B, 상대 RR과 Established(3세션) | - | 미확인 |
| ISP-1-PE-A, PE-B | `show ip bgp summary` | P1, P2와 iBGP Established(AS 65100) | - | 미확인 |
| ISP-1-P1, P2 | `show ip bgp neighbors \| include reflector` | PE-A, PE-B가 Route-Reflector Client | - | 미확인 |
| ISP-1-전체 | `show ip bgp neighbors \| include hold` | iBGP 세션 timers 3/9 | - | 미확인 |

## 2. 사이트 eBGP 세션 (KAN-18, 2주 월)

- 사이트 쪽 확인은 `*/core/isp-link.md`. 이 절은 PE 쪽 확인.

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-1-PE-A | `show ip bgp summary` | HQ-R1(100.1.11.1), DC-R3(100.1.21.1)와 eBGP Established | - | 미확인 |
| ISP-1-PE-B | `show ip bgp summary` | BR1-R1(100.1.31.1), BR2-R1(100.1.41.1)와 eBGP Established | - | 미확인 |
| ISP-1-PE-A, PE-B | `show ip bgp neighbors <사이트 주소> advertised-routes` | 사이트에는 기본 경로만 전달(`default-originate`, `pl-deny-all`) | - | 미확인 |
| ISP-1-PE-A, PE-B | `show ip bgp neighbors <사이트 주소> routes` | 해당 사이트 /16만 수신(`pl-<사이트>-in`) | - | 미확인 |
| ISP-1-PE-A, PE-B | `show ip bgp neighbors <사이트 주소> \| include hold` | eBGP 세션 timers 3/9 | - | 미확인 |

## 3. PE 정책과 사이트 간 경로 (KAN-17, 3주 목)

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-1-PE-A, PE-B | `show ip bgp` | 사이트 /16을 eBGP로 수신하고 상대 PE의 사이트 /16을 RR 경유로 수신(next-hop은 상대 PE Loopback0, `next-hop-self`) | - | 미확인 |
| ISP-1-P1, P2 | `show ip bgp` | 모든 사이트 /16 반사, `Originator`, `Cluster list` 표시 | - | 미확인 |
