# 서버 VLAN ACL 검증 (DC / distribution)

- Jira: KAN-26
- 대상 장비: DC-SW3-L3, DC-SW4-L3
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 5절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-SW3-L3, DC-SW4-L3 | `show ip access-lists dns-only` | DNS(UDP/TCP 53) 허용 4항목 + deny 항목, 카운트 증가 | - | 미확인 |
| 호스트 | `nslookup dc-r1.lab.local 10.2.50.10` | 10.10.10.1 응답 | - | 미확인 |
| 호스트 | `서버 VLAN 대상 DNS 외 포트 접속 시도` | 차단 | - | 미확인 |
| 호스트 | `ping 10.2.50.10` | ICMP 차단(ACL 적용 후 정상) | - | 미확인 |
