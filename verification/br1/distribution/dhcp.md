# DHCP 검증 (BR1 / distribution)

- Jira: KAN-14
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 3절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-SW1-L3, BR1-SW2-L3 | `show ip dhcp pool` | 풀 범위 Active `.100`~`.163`, Standby `.164`~`.227` | - | 미확인 |
| BR1-SW1-L3, BR1-SW2-L3 | `show ip dhcp binding` | 호스트에 풀 범위 주소 임대, 게이트웨이 VIP, DNS 10.2.50.10 / .11 | - | 미확인 |
| 호스트 | `udhcpc -i eth0, ip addr` | 풀 범위 주소와 `resolv.conf` DNS 두 대 | - | 미확인 |
