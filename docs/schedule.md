# 일정 — 1인 체제, 3.5주

기간: 2026-10-12(월) ~ 2026-11-04(수, 반일). 평일 17.5일이며 6주(30일) 계획을 압축한 일정.

## 역할 분담

담당자: 남기석(전 사이트). 작업 영역은 `Net-Cloud`(DC, HQ, ISP-1, ISP-2, Private Internet, DNS, QoS)와 `Policy`(BR1, BR2, DMVPN, 접근통제 ACL)로 구분하며 Jira 라벨도 동일하게 사용. 일별 표의 `/` 앞은 Net-Cloud, 뒤는 Policy 영역 작업.

⚠️ 2인 일정을 1인이 수행하도록 통합한 일정으로 하루 작업량이 두 배. 지연 시 대응은 아래 "리스크"의 컷 순서 참조.

## VLAN 태그

| VLAN | 태그 |
|---|---|
| 10 | `vlan-hq-public` |
| 20 | `vlan-hq-private` |
| 50 | `vlan-dc-server` |
| BR1 10 | `vlan-br1-public` |
| BR1 20 | `vlan-br1-private` |
| BR2 10 | `vlan-br2-public` |
| BR2 20 | `vlan-br2-private` |
| 100 | `vlan-mgmt` |
| 999 | `vlan-native` |

## 일별 목표

| 주 | 일 | 작업 |
|---|---|---|
| 1주 (10/12~10/16) | 월 | AS/IP/VLAN 표 리뷰, GNS3/Jira/Slack/Git 세팅 |
|  | 화 | 7200/cEOS/IOU RAM 실측, 노드 배치, 기본 링크 up/up / BR1+BR2 노드 배치, 기본 링크 up/up |
|  | 수 | DC VLAN/트렁크(Arista), DC VRRP / BR1 VLAN/트렁크, VRRP |
|  | 목 | HQ VLAN/트렁크, VRRP, Po1-3 / BR2 VLAN/트렁크, VRRP |
|  | 금 | 사이트 내부 OSPF, 사이트 DHCP(HQ), DC/HQ 내부통신 검증 / 사이트 내부 OSPF, 사이트 DHCP(BR1, BR2), BR1/BR2 내부통신 검증 |
| 2주 (10/19~10/23) | 월 | DC/HQ↔ISP 정적 기본 경로+OSPF 기본 경로 광고, PI 기본연결+OSPF(프로세스2) / BR1/BR2↔ISP 정적경로+track |
|  | 화 | PI ACL 화이트리스트, DMVPN Hub(DC-R2) crypto / DMVPN Spoke 사전 준비 |
|  | 수 | DMVPN Spoke(HQ) / DMVPN Spoke(BR1, BR2) |
|  | 목 | DC-DNS-1·DC-DNS-2(Alpine) 배치+검증, 호스트 DHCP 할당 확인 / Phase3 shortcut 검증 |
|  | 금 | DNS 서버 VLAN ACL, 전 사이트 상호도달 확인 / 통합 테스트 |
| 3주 (10/26~10/30) | 월 | VRRP 장애 전환, EtherChannel 멤버 다운 실측 |
|  | 화 | DMVPN Hub 장애 실측 |
|  | 수 | ISP-1, ISP-2 내부 eBGP 8세션 |
|  | 목 | PE 고객 대역 광고, ISP-1 장애→ISP-2/DMVPN 전환 실측 |
|  | 금 | QoS 20포트(DC/HQ 12대, BR 8대), 트러블슈팅 로그 정리 |
| 3.5주 (11/2~11/4) | 월 | PPT(DC/HQ/ISP/이중화) / PPT(BR/DMVPN) |
|  | 화 | PPT 마무리, 리허설 |
|  | 수(반일) | 예비 |

## Jira 이슈 표기

Jira 이슈 제목 앞의 `[1주]`, `[2주]`, `[3주]`, `[3.5주]`는 이 일정의 주차이며 마감일(Due date)이 속한 주 기준. 이슈 설명 끝의 `일정:`에 위 일별 목표의 요일을 표기.

| 이슈 표기 | 기간 | 이슈 수 |
|---|---|---|
| [1주] | 10/12 ~ 10/16 | 11 |
| [2주] | 10/19 ~ 10/23 | 9 |
| [3주] | 10/26 ~ 10/30 | 9 |
| [3.5주] | 11/2 ~ 11/4 | 1 |

일정표에는 있으나 별도 이슈가 없는 작업(전 사이트 상호도달 확인, 통합 테스트)은 관련 이슈 설명에 포함. 이슈별 마감일은 Jira Due date와 `jira/jira-tickets-3person.csv`의 `Due Date` 열에 동일하게 기재.

## 리스크
- 2인용 일정을 1인이 수행하여 병렬 작업이 직렬로 전환되고, 하루 작업량이 두 배가 되어 예비일이 사실상 없음.
- 위험 구간: 2주(PI·DMVPN), 3주 수~금(eBGP, QoS). eBGP와 QoS는 일정 후반에 배치하여 앞 단계(OSPF, DHCP, DMVPN)를 먼저 확보.
- 지연 발생 시 컷 순서(앞에서부터 생략): ① QoS 스위치 검증 생략(라우터만) ② BR2 구성은 BR1과 같은 패턴을 복제만 하고 장애 실측 생략 ③ ISP-2 생략, ISP-1+DMVPN만 ④ DC-DNS-2(백업) 서버 구축 생략, DC-DNS-1만 운영
- 일정 연장이 가능하면 위 컷 순서의 생략 항목을 줄이는 방향으로 조정.
