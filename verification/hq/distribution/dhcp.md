# DHCP 검증 (HQ / distribution)

- Jira: KAN-14
- 대상 장비: HQ-SW-2(Active), HQ-SW-3(Standby)
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 3절
- 검증일: 2026-10-13

| 장비                                | 명령                        | 기대 결과                                            | 실제 결과 | 판정  |
| --------------------------------- | ------------------------- | ------------------------------------------------ | ----- | --- |
| HQ-SW-2(Active), HQ-SW-3(Standby) | `show ip dhcp pool` | 풀 범위 Active `.100`~`.163`, Standby `.164`~`.227` | 두 스위치 모두 풀 hq-public, hq-private, 각 Total 254, Excluded 190(사용 가능 64개), 제외 구간 설정 반영. 캡처 시점에는 임대 0 | 통과 |
| HQ-SW-2(Active), HQ-SW-3(Standby) | `show ip dhcp binding` | 호스트에 풀 범위 주소 임대, 게이트웨이 VIP, DNS 10.2.50.10 / .11 | HQ-SW-2에 10.1.10.100(Vlan10, HQ-PC1), HQ-SW-3에 10.1.20.164(Vlan20, HQ-PC2) 임대. 클라이언트 ID가 호스트 MAC과 일치, 임대 만료 24시간 후 | 통과 |
| 호스트 | `udhcpc -i eth0`, `ip addr`, `cat /etc/resolv.conf` | 풀 범위 주소와 `resolv.conf` DNS 두 대 | HQ-PC1 10.1.10.100(서버 10.1.10.2, Active 범위), HQ-PC2 10.1.20.164(서버 10.1.20.3, Standby 범위), lease time 86400. 기본 경로 10.1.10.1, 10.1.20.1(VIP), resolv.conf search lab.local, nameserver 10.2.50.10과 10.2.50.11 | 통과 |

HQ-SW-2 DHCP POOL
![HQ](images/Pasted%20image%2020261013194122.png)

HQ-SW-2 DHCP BINDING
![HQ](images/Pasted%20image%2020261013194732.png)

HQ-SW-3 DHCP POOL
![HQ](images/Pasted%20image%2020261013194237.png)

HQ-SW-3 DHCP BINDING
![HQ](images/Pasted%20image%2020261013194743.png)

HQ-PC1
![HQ](images/Pasted%20image%2020261013194518.png)

![HQ](images/Pasted%20image%2020261013194956.png)

HQ-PC2
![HQ](images/Pasted%20image%2020261013194442.png)

![HQ](images/Pasted%20image%2020261013195022.png)
