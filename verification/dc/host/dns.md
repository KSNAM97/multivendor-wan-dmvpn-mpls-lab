# DNS 서버 검증 (DC / host)

- Jira: KAN-24
- 대상 장비: DC-DNS-1, DC-DNS-2
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 2절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-DNS-1 | `nslookup dc-r1.lab.local 10.2.50.10` | 10.10.10.1 | - | 미확인 |
| DC-DNS-2 | `nslookup dc-r1.lab.local 10.2.50.11` | 10.10.10.1 | - | 미확인 |
| 호스트 | `주 서버 중지 후 nslookup dc-r1.lab.local 10.2.50.11` | 백업 서버가 같은 응답 | - | 미확인 |
| DC-DNS-1 | `dnsmasq 로그(`log-queries`)` | 질의 기록 | - | 미확인 |
