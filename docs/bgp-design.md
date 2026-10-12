# BGP 설계

ISP-1, ISP-2 내부의 iBGP Route Reflector 구성을 정리한 문서. 사이트(HQ, DC, BR1, BR2)와 Private Internet은 BGP 미사용. ISP-1과 ISP-2는 서로 연결되지 않으며 eBGP 세션 없음. 링크 IP는 `docs/ip-mapping.md`, 포트는 `docs/port-mapping.md` 기준.

## 1. AS 구성

- ISP마다 단일 AS를 사용하는 iBGP 구성. P1, P2가 Route Reflector(RR), PE-A, PE-B가 RR Client.
- P1, P2는 같은 `bgp cluster-id`를 사용하는 이중 RR. Client는 P1, P2 양쪽과 세션을 맺어 RR 한 대 장애 시에도 경로 유지.
- 사이트 AS(HQ 65001, DC 65002, BR1 65003, BR2 65004)는 사이트 식별용 번호이며 BGP 세션 없음. 사이트↔ISP 구간은 정적 경로와 OSPF 재분배(`docs/ospf-design.md` 6절).

| ISP | AS | 대역 | RR (P1, P2) | RR Client | cluster-id |
|---|---|---|---|---|---|
| ISP-1 | 65100 | 100.1.0.0/16 | ISP-1-P1, ISP-1-P2 | ISP-1-PE-A, ISP-1-PE-B | 1.1.1.1 |
| ISP-2 | 65200 | 100.2.0.0/16 | ISP-2-P1, ISP-2-P2 | ISP-2-PE-A, ISP-2-PE-B | 2.2.2.2 |

## 2. Router ID (Loopback0)

- 형식: ISP-1 `100.100.100.<n>`, ISP-2 `200.200.200.<n>`. 번호는 PE-A 1, P1 2, P2 3, PE-B 4. Loopback0 `/32`와 `bgp router-id`에 같은 값 사용.
- iBGP 세션 주소와 `update-source`로 사용하며, ISP 내부 OSPF로 서로 도달.

| 장비 | 역할 | Router ID / Loopback0 |
|---|---|---|
| ISP-1-PE-A | RR Client | 100.100.100.1/32 |
| ISP-1-P1 | RR | 100.100.100.2/32 |
| ISP-1-P2 | RR | 100.100.100.3/32 |
| ISP-1-PE-B | RR Client | 100.100.100.4/32 |
| ISP-2-PE-A | RR Client | 200.200.200.1/32 |
| ISP-2-P1 | RR | 200.200.200.2/32 |
| ISP-2-P2 | RR | 200.200.200.3/32 |
| ISP-2-PE-B | RR Client | 200.200.200.4/32 |

## 3. ISP 내부 OSPF (iBGP 도달성)

- ISP 내부 링크와 Loopback0 도달성 확보용. process 1, Area 0, router-id는 Loopback0 값.
- 대상: ISP 내부 링크(링크 # 71~75, 76~80)와 Loopback0. 사이트 방향 링크(PE–사이트)는 포함하지 않음.
- 사이트 방향 인터페이스는 `passive` 불필요(OSPF 미활성). Loopback0는 `ip ospf 1 area 0`으로 광고.

## 4. iBGP 세션

- ISP당 5세션, 합계 10세션. Loopback0 주소로 세션을 맺고 `neighbor <peer-lo0> update-source loopback0` 적용.
- P1–P2는 RR끼리 일반 iBGP(non-client) 피어. PE는 RR 두 대와만 세션을 맺으며 PE끼리는 세션 없음.
- RR 쪽 설정: `neighbor <PE-lo0> route-reflector-client`.

| # | 링크 # | RR 장비 | RR Loopback0 | 상대 장비 | 상대 Loopback0 | 관계 |
|---|---|---|---|---|---|---|
| 1 | 71 | ISP-1-P1 | 100.100.100.2 | ISP-1-PE-A | 100.100.100.1 | Client |
| 2 | 72 | ISP-1-P2 | 100.100.100.3 | ISP-1-PE-A | 100.100.100.1 | Client |
| 3 | 73 | ISP-1-P1 | 100.100.100.2 | ISP-1-PE-B | 100.100.100.4 | Client |
| 4 | 74 | ISP-1-P2 | 100.100.100.3 | ISP-1-PE-B | 100.100.100.4 | Client |
| 5 | 75 | ISP-1-P1 | 100.100.100.2 | ISP-1-P2 | 100.100.100.3 | RR 간 피어 |
| 6 | 76 | ISP-2-P1 | 200.200.200.2 | ISP-2-PE-A | 200.200.200.1 | Client |
| 7 | 77 | ISP-2-P2 | 200.200.200.3 | ISP-2-PE-A | 200.200.200.1 | Client |
| 8 | 78 | ISP-2-P1 | 200.200.200.2 | ISP-2-PE-B | 200.200.200.4 | Client |
| 9 | 79 | ISP-2-P2 | 200.200.200.3 | ISP-2-PE-B | 200.200.200.4 | Client |
| 10 | 80 | ISP-2-P1 | 200.200.200.2 | ISP-2-P2 | 200.200.200.3 | RR 간 피어 |

## 5. 광고 prefix

- PE 장비가 연결된 사이트의 /16을 `network` 구문으로 광고. 사이트 prefix는 PE의 정적 경로(사이트 라우터 방향)로 라우팅 테이블에 존재.
- RR(P1, P2)은 prefix를 광고하지 않고 반사만 수행. 반사된 경로의 next-hop은 광고한 PE의 Loopback0.

| PE 장비 | 광고 prefix | 정적 경로 next-hop(사이트 라우터) | 링크 # |
|---|---|---|---|
| ISP-1-PE-A | 10.1.0.0/16 | HQ-R1 100.1.11.1 | 6 |
| ISP-2-PE-A | 10.1.0.0/16 | HQ-R2 100.2.12.1 | 7 |
| ISP-1-PE-A | 10.2.0.0/16 | DC-R3 100.1.21.1 | 33 |
| ISP-2-PE-A | 10.2.0.0/16 | DC-R4 100.2.22.1 | 34 |
| ISP-1-PE-B | 10.3.0.0/16 | BR1-R1 100.1.31.1 | 48 |
| ISP-2-PE-B | 10.3.0.0/16 | BR1-R2 100.2.32.1 | 49 |
| ISP-1-PE-B | 10.4.0.0/16 | BR2-R1 100.1.41.1 | 62 |
| ISP-2-PE-B | 10.4.0.0/16 | BR2-R2 100.2.42.1 | 63 |

## 6. 타이머와 공통 설정

- `timers bgp 3 9`(keepalive 3초, hold-time 9초). ISP 장애 시 ISP-2 전환 실측의 수렴 시간을 단축하기 위한 값.
- `bgp log-neighbor-changes`, `no auto-summary`, `no synchronization`(해당 시) 적용.
- route-map, prefix-list, 인증은 적용하지 않음(실습 환경).

## 7. 경로 선택과 이중화

- 사이트는 ISP-1과 ISP-2에 각각 연결되며 정적 기본 경로를 OSPF로 광고할 때의 metric(ISP-1 100, ISP-2 200)으로 주/보조 경로 구분(`docs/ospf-design.md` 6절).
- ISP 내부는 RR 이중화로 P1 또는 P2 한 대가 내려가도 나머지 RR을 통해 경로 유지.
- ISP-1 장애 시 IP SLA/track이 정적 기본 경로를 제거하여 ISP-2 경로로 전환. 이 전환 시간이 BGP hold-time 9초의 영향을 받음.
- DMVPN 터널(Private Internet 경유)은 ISP와 독립된 경로이며 `docs/dmvpn-design.md` 참조.
