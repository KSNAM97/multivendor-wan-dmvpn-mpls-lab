# BGP 검증(iBGP RR, 사이트 eBGP) (ISP2 / core)

- Jira: KAN-16, KAN-17
- 대상 장비: ISP-2-PE-A, P1, P2, PE-B
- 기준 문서: [bgp-design.md](../../../docs/bgp-design.md) 2~6절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| ISP-2-PE-A, PE-B, P1, P2 | `show ip ospf neighbor` | ISP 내부 이웃 Full(point-to-point), Loopback0 `200.200.200.1~4` 도달 | - | 미확인 |
| ISP-2-P1, P2 | `show ip bgp summary` | AS 65200, PE-A, PE-B, 상대 RR과 Established(3세션) | - | 미확인 |
| ISP-2-PE-A, PE-B | `show ip bgp summary` | P1, P2(iBGP)와 사이트 라우터 2대(eBGP, PE-A는 HQ-R2, DC-R4, PE-B는 BR1-R2, BR2-R2)와 Established | - | 미확인 |
| ISP-2-P1, P2 | `show ip bgp neighbors \| include reflector` | PE-A, PE-B가 Route-Reflector Client | - | 미확인 |
| ISP-2-PE-A, PE-B | `show ip bgp` | 사이트 /16 prefix를 eBGP로 수신(입력 prefix-list), 상대 PE의 prefix를 RR 경유로 수신 | - | 미확인 |
| ISP-2-PE-A, PE-B | `show ip bgp neighbors <사이트 주소> advertised-routes` | 사이트에는 기본 경로만 전달(`default-originate`) | - | 미확인 |
| 전체 | `show ip bgp neighbors \| include hold` | timers 3/9 | - | 미확인 |
