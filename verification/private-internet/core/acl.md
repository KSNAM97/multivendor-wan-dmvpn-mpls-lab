# Private Internet ACL 검증

- Jira: KAN-19
- 대상 장비: Private-net
- 기준 문서: [dmvpn-design.md](../../../docs/dmvpn-design.md) 5절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| Private-net | `show ip access-lists pi-whitelist` | OSPF, GRE, ESP, UDP 500(isakmp), ICMP 허용 항목 카운트 증가 | - | 미확인 |
| Private-net | `show ip interface | include access list` | 5개 링크 인터페이스에 `pi-whitelist` 입력 방향 적용 | - | 미확인 |
| 사이트 라우터 | `ping(차단 대상 프로토콜)` | 화이트리스트 외 트래픽 차단, 카운트 증가 | - | 미확인 |

`permit icmp any any`는 임시 항목이며 검증 후 제거.
