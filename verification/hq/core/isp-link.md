# ISP 연결 검증 (HQ / core)

- Jira: KAN-18
- 대상 장비: HQ-R1(ISP-1, metric 100), HQ-R2(ISP-2, metric 200)
- 기준 문서: [ospf-design.md](../../../docs/ospf-design.md) 6절, [bgp-design.md](../../../docs/bgp-design.md) 5~6절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| HQ-R1(ISP-1, metric 100), HQ-R2(ISP-2, metric 200) | `show ip bgp summary` | ISP PE와 Established(ISP AS 65100 또는 65200) | - | 미확인 |
| HQ-R1(ISP-1, metric 100), HQ-R2(ISP-2, metric 200) | `show ip route 0.0.0.0` | BGP 기본 경로 `B*`(PE 방향) | - | 미확인 |
| HQ-R1(ISP-1, metric 100), HQ-R2(ISP-2, metric 200) | `show ip bgp neighbors <PE 주소> advertised-routes` | 사이트 /16만 광고(`rm-isp-out`) | - | 미확인 |
| HQ-R1(ISP-1, metric 100), HQ-R2(ISP-2, metric 200) | `show route-map`, `show ip prefix-list` | `rm-isp-in`, `rm-isp-out`, `pl-default`, `pl-site` 적용 | - | 미확인 |
| HQ-R1(ISP-1, metric 100), HQ-R2(ISP-2, metric 200) | `show ip ospf database external` | `default-information originate` 기본 경로 Type 1 광고 | - | 미확인 |

장애 시 BGP 세션이 내려가 기본 경로가 철회되고 OSPF 기본 경로 광고가 ISP-2로 전환되는지는 [scenarios/](../../scenarios/)에서 확인.
