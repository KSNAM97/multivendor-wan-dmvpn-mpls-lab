# BGP(iBGP RR) 검증 (ISP1 / core)

- Jira: KAN-15, KAN-17
- 대상 장비: ISP-1-PE-A, P1, P2, PE-B
- 기준 문서: [bgp-design.md](../../../docs/bgp-design.md) 2~5절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-1-PE-A, PE-B, P1, P2 | `show ip ospf neighbor` | ISP 내부 이웃 Full, Loopback0 `100.100.100.1~4` 도달 | - | 미확인 |
| ISP-1-P1, P2 | `show ip bgp summary` | AS 65100, PE-A, PE-B, 상대 RR과 Established(3세션) | - | 미확인 |
| ISP-1-PE-A, PE-B | `show ip bgp summary` | P1, P2와 Established(AS 65100) | - | 미확인 |
| ISP-1-P1, P2 | `show ip bgp neighbors | include reflector` | PE-A, PE-B가 Route-Reflector Client | - | 미확인 |
| ISP-1-PE-A, PE-B | `show ip bgp` | 사이트 /16 prefix 광고(정적 경로 + `network`), 상대 PE의 prefix를 RR 경유로 수신 | - | 미확인 |
| 전체 | `show ip bgp neighbors | include hold` | timers 3/9 | - | 미확인 |
