# BGP 검증(iBGP RR, 사이트 eBGP) (ISP1 / core)

- Jira: KAN-15, KAN-16(iBGP), KAN-17(PE 정책), KAN-18(사이트 eBGP)
- 대상 장비: ISP-1-PE-A, P1, P2, PE-B
- 기준 문서: [bgp-design.md](../../../docs/bgp-design.md) 2~6절
- 검증일: 2026-10-17

## 1. ISP 내부 OSPF와 iBGP (KAN-15, 16, 3주 수)

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-1-PE-A, PE-B, P1, P2 | `show ip ospf neighbor` | ISP 내부 이웃 Full(point-to-point), Loopback0 `100.100.100.1~4` 도달 | 모두 FULL/ -(Pri 0). P1은 100.100.100.1(PE-A), .4(PE-B), .3(P2) 3개, P2는 .1, .4, .2 3개, PE-A와 PE-B는 P1, P2 2개 | 통과 |
| ISP-1-P1, P2 | `show ip bgp summary` | AS 65100, PE-A, PE-B, 상대 RR과 Established(3세션) | AS 65100, P1(router-id 100.100.100.2)과 P2(100.100.100.3) 모두 이웃 3개(100.100.100.1, .3 또는 .2, .4)가 Up/Down 약 1시간 28분 이상 유지, State/PfxRcd 숫자(Established) | 통과 |
| ISP-1-PE-A, PE-B | `show ip bgp summary` | P1, P2와 iBGP Established(AS 65100) | PE-A(100.100.100.1)와 PE-B(100.100.100.4) 모두 P1(100.100.100.2), P2(100.100.100.3) 2세션 Established | 통과 |
| ISP-1-P1, P2 | `show ip bgp neighbors \| include Reflector` | PE-A, PE-B가 Route-Reflector Client | P1, P2 모두 `Route-Reflector Client` 2건(PE-A, PE-B). 대소문자를 구분하므로 `Reflector`로 필터 | 통과 |
| ISP-1 전체 | `show ip bgp neighbors \| include hold` | iBGP 세션 timers 3/9 | P1, P2는 이웃 3개, PE-A, PE-B는 이웃 2개 모두 `Configured hold time is 9, keepalive interval is 3 seconds` | 통과 |

ISP-1-P1 OSPF neighbor
![ISP-1-P1 OSPF neighbor](images/Pasted%20image%2020261017203736.png)

ISP-1-PE-A OSPF neighbor
![ISP-1-PE-A OSPF neighbor](images/Pasted%20image%2020261017203749.png)

ISP-1-PE-B OSPF neighbor
![ISP-1-PE-B OSPF neighbor](images/Pasted%20image%2020261017203800.png)

ISP-1-P2 OSPF neighbor
![ISP-1-P2 OSPF neighbor](images/Pasted%20image%2020261017203816.png)

ISP-1-P1 BGP summary
![ISP-1-P1 BGP summary](images/Pasted%20image%2020261017203848.png)

ISP-1-P2 BGP summary
![ISP-1-P2 BGP summary](images/Pasted%20image%2020261017203925.png)

ISP-1-PE-A BGP summary
![ISP-1-PE-A BGP summary](images/Pasted%20image%2020261017203904.png)

ISP-1-PE-B BGP summary
![ISP-1-PE-B BGP summary](images/Pasted%20image%2020261017203915.png)

ISP-1-P1 BGP neighbors Reflector
![ISP-1-P1 BGP neighbors Reflector](images/Pasted%20image%2020261017204145.png)

ISP-1-P2 BGP neighbors Reflector
![ISP-1-P2 BGP neighbors Reflector](images/Pasted%20image%2020261017204158.png)

ISP-1-P1 BGP neighbors hold
![ISP-1-P1 BGP neighbors hold](images/Pasted%20image%2020261017204230.png)

ISP-1-PE-A BGP neighbors hold
![ISP-1-PE-A BGP neighbors hold](images/Pasted%20image%2020261017204239.png)

ISP-1-PE-B BGP neighbors hold
![ISP-1-PE-B BGP neighbors hold](images/Pasted%20image%2020261017204247.png)

ISP-1-P2 BGP neighbors hold
![ISP-1-P2 BGP neighbors hold](images/Pasted%20image%2020261017204259.png)

## 2. 사이트 eBGP 세션 (KAN-18, 2주 월)

- 사이트 쪽 확인은 `*/core/isp-link.md`. 이 절은 PE 쪽 확인(VRF `corp` 안의 세션, `docs/mpls-design.md` 3절).

| 장비               | 명령                                                                | 기대 결과                                                    | 실제 결과 | 판정  |
| ---------------- | ----------------------------------------------------------------- | -------------------------------------------------------- | ----- | --- |
| ISP-1-PE-A | `show ip bgp vpnv4 vrf corp summary` | HQ-R1(100.1.11.1), DC-R3(100.1.21.1)와 eBGP Established | HQ-R1(100.1.11.1, AS 65001) Up/Down 00:42:03, DC-R3(100.1.21.1, AS 65002) Up/Down 00:12:45 모두 Established, PfxRcd 각 1 | 통과 |
| ISP-1-PE-B       | `show ip bgp vpnv4 vrf corp summary`                              | BR1-R1(100.1.31.1), BR2-R1(100.1.41.1)와 eBGP Established | BR1-R1(100.1.31.1, AS 65003) Up/Down 00:00:47, BR2-R1(100.1.41.1, AS 65004) Up/Down 00:00:33 모두 Established, PfxRcd 각 1 | 통과 |
| ISP-1-PE-A, PE-B | `show ip bgp vpnv4 vrf corp neighbors <사이트 주소> advertised-routes` | 사이트에는 기본 경로만 전달(`default-originate`, `pl-deny-all`) | PE-A는 HQ-R1, DC-R3 모두 `Originating default network 0.0.0.0`만 전송(일반 prefix 0개). PE-B는 BR1-R1, BR2-R1 모두 `Originating default network 0.0.0.0`만 전송(일반 prefix 0개) | 통과 |
| ISP-1-PE-A, PE-B | `show ip bgp vpnv4 vrf corp neighbors <사이트 주소> routes` | 해당 사이트 /16만 수신(`pl-<사이트>-in`) | PE-A는 HQ-R1 이웃에서 `10.1.0.0/16`(Path 65001 i), DC-R3 이웃에서 `10.2.0.0/16`(Path 65002 i) 각 1개만 수신, RD 65100:1. PE-B는 BR1-R1 이웃에서 `10.3.0.0/16`(Path 65003 i), BR2-R1 이웃에서 `10.4.0.0/16`(Path 65004 i) 각 1개만 수신, RD 65100:1 | 통과 |
| ISP-1-PE-A, PE-B | `show ip bgp vpnv4 vrf corp neighbors <사이트 주소> \| include hold`   | eBGP 세션 timers 3/9                                       | PE-A는 HQ-R1, DC-R3 이웃 모두 hold time 9, keepalive 3초(Configured도 동일). PE-B는 BR1-R1, BR2-R1 이웃 모두 hold time 9, keepalive 3초(Configured도 동일) | 통과 |

ISP-1-PE-A VRF summary
![ISP-1-PE-A VRF summary](images/site-ebgp-pe-a-summary.webp)

ISP-1-PE-A HQ-R1 routes
![ISP-1-PE-A HQ-R1 routes](images/site-ebgp-pe-a-hq-routes.webp)

ISP-1-PE-A HQ-R1 advertised-routes
![ISP-1-PE-A HQ-R1 advertised-routes](images/site-ebgp-pe-a-hq-advertised.webp)

ISP-1-PE-A HQ-R1 hold
![ISP-1-PE-A HQ-R1 hold](images/site-ebgp-pe-a-hq-hold.webp)

ISP-1-PE-A DC-R3 routes
![ISP-1-PE-A DC-R3 routes](images/site-ebgp-pe-a-dc-routes.webp)

ISP-1-PE-A DC-R3 advertised-routes
![ISP-1-PE-A DC-R3 advertised-routes](images/site-ebgp-pe-a-dc-advertised.webp)

ISP-1-PE-A DC-R3 hold
![ISP-1-PE-A DC-R3 hold](images/site-ebgp-pe-a-dc-hold.webp)

ISP-1-PE-B VRF summary
![ISP-1-PE-B VRF summary](images/site-ebgp-pe-b-summary.webp)

ISP-1-PE-B BR1-R1 routes
![ISP-1-PE-B BR1-R1 routes](images/site-ebgp-pe-b-br1-routes.webp)

ISP-1-PE-B BR1-R1 advertised-routes
![ISP-1-PE-B BR1-R1 advertised-routes](images/site-ebgp-pe-b-br1-advertised.webp)

ISP-1-PE-B BR1-R1 hold
![ISP-1-PE-B BR1-R1 hold](images/site-ebgp-pe-b-br1-hold.webp)

ISP-1-PE-B BR2-R1 routes
![ISP-1-PE-B BR2-R1 routes](images/site-ebgp-pe-b-br2-routes.webp)

ISP-1-PE-B BR2-R1 advertised-routes
![ISP-1-PE-B BR2-R1 advertised-routes](images/site-ebgp-pe-b-br2-advertised.webp)

ISP-1-PE-B BR2-R1 hold
![ISP-1-PE-B BR2-R1 hold](images/site-ebgp-pe-b-br2-hold.webp)

## 3. PE 정책과 사이트 간 경로 (KAN-17, 3주 목)

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-1-PE-A, PE-B | `show ip bgp vpnv4 all` | 사이트 /16을 eBGP로 수신하고 상대 PE의 사이트 /16을 RR 경유로 수신(next-hop은 상대 PE Loopback0, `next-hop-self`) | - | 미확인 |
| ISP-1-P1, P2 | `show ip bgp vpnv4 all` | 모든 사이트 /16 반사, `Originator`, `Cluster list` 표시 | - | 미확인 |
