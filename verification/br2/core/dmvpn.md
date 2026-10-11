# DMVPN Spoke 검증 (BR2 / core)

- Jira: KAN-23
- 대상 장비: BR2-R2
- 기준 문서: [dmvpn-design.md](../../../docs/dmvpn-design.md) 1~4절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-R2 | `show dmvpn` | Hub(172.16.0.1) Up, Tunnel0 172.16.0.12, NBMA 203.0.113.17 | - | 미확인 |
| BR2-R2 | `show ip ospf neighbor` | Tunnel0에서 DC-R2와 FULL(Spoke priority 0) | - | 미확인 |
| BR2-R2 | `show crypto isakmp sa` | Hub와 QM_IDLE | - | 미확인 |
| BR2-R2 | `ping 다른 Spoke 사이트 호스트(2회 이상)` | Phase 3 shortcut 생성, `show ip nhrp`에 Spoke 간 dynamic 항목, `traceroute`가 Hub를 거치지 않음 | - | 미확인 |

Phase 3 shortcut 검증은 2주 금(10/23)에 수행.
