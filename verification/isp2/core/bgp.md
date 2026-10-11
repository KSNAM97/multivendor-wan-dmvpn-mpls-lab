# eBGP 검증 (ISP2 / core)

- Jira: KAN-16, KAN-17
- 대상 장비: ISP-2-PE-A, P1, P2, PE-B
- 기준 문서: [bgp-design.md](../../../docs/bgp-design.md) 2~4절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-2-PE-A | `show ip bgp summary` | P1, P2와 Established(AS 65121~65124) | - | 미확인 |
| ISP-2-PE-B | `show ip bgp summary` | P1, P2와 Established | - | 미확인 |
| ISP-2-P1, P2 | `show ip bgp summary` | PE-A, PE-B와 Established(P1–P2 간 세션은 없음) | - | 미확인 |
| ISP-2-PE-A, PE-B | `show ip bgp` | 사이트 /16 prefix 광고(정적 경로 + `network`) | - | 미확인 |
| 전체 | `show ip bgp neighbors | include hold` | timers 3/9 | - | 미확인 |
