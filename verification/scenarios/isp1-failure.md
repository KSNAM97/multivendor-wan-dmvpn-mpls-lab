# ISP-1 장애 시 ISP-2, DMVPN 전환

- Jira: KAN-29
- 대상 장비: ISP-1 PE-A, HQ-R1, DC-R3, BR1, BR2 및 DMVPN
- 기준 문서: [bgp-design.md](../../docs/bgp-design.md), [ospf-design.md](../../docs/ospf-design.md) 6절
- 검증일: -

| 순서 | 동작 | 기대 결과 | 측정값 | 판정 |
|---|---|---|---|---|
| 1 | ISP-1 장애 유발(링크 `shutdown` 또는 PE-A 중지) | BGP hold-time(9초) 이내에 세션 다운 감지 | - | 미확인 |
| 2 | 사이트 엣지 라우터의 BGP 세션, 기본 경로 | PE와 세션 Down, BGP 기본 경로 `B*` 제거 | - | 미확인 |
| 3 | 기본 경로 광고 | `default-information originate`가 ISP-2 쪽(metric 200)으로 전환 | - | 미확인 |
| 4 | 사이트 간 통신 | DMVPN(Private Internet) 경로로 유지 | - | 미확인 |
| 5 | 복구 | ISP-1 경로로 복귀 | - | 미확인 |
