# DHCP 검증 (BR1 / distribution)

- Jira: KAN-14
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 3절
- 검증일: 2026-10-17
- Aruba CX 교체: BR1 스위치를 Aruba CX(ArubaOS-CX Virtual 10.07)로 교체하고 AOS-CX 명령 기준으로 재검증

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-SW1-L3 | `show dhcp-server` | 풀 범위 Active `.100`~`.163` | enabled, operational. br1-public 10.3.10.100~.163, br1-private 10.3.20.100~.163, prefix 24, Lease Duration 01:00:00, 옵션 3(10.3.10.1, 10.3.20.1), 6(10.2.50.10 10.2.50.11), 15(lab.local) | 통과 |
| BR1-SW2-L3 | `show dhcp-server` | 풀 범위 Standby `.164`~`.227` | enabled, operational. br1-public 10.3.10.164~.227, br1-private 10.3.20.164~.227, prefix 24, Lease Duration 01:00:00, 옵션 3, 6, 15는 SW1과 동일 | 통과 |
| BR1-SW1-L3 | `show dhcp-server leases` | 호스트에 풀 범위 주소 임대 | 10.3.10.153(02:42:a2:4e:37:00), 10.3.20.117(02:42:b8:51:88:00) 임대, 클라이언트 ID가 호스트 MAC과 일치, 만료 18/10/2026 09:40(24시간 후) | 통과 |
| BR1-SW2-L3 | `show dhcp-server leases` | 먼저 응답한 SW1이 할당하므로 바인딩 없음 | No DHCP leases in the database | 통과 |
| BR1-PC1 | `udhcpc -i eth0`, `ip addr`, `cat /etc/resolv.conf` | 풀 범위 주소와 `resolv.conf` DNS 두 대 | 10.3.10.153/24(서버 10.3.10.2, lease time 86400), resolv.conf search lab.local, nameserver 10.2.50.10과 10.2.50.11 | 통과 |
| BR1-PC2 | `udhcpc -i eth0`, `ip addr`, `cat /etc/resolv.conf` | 풀 범위 주소와 `resolv.conf` DNS 두 대 | 10.3.20.117/24(서버 10.3.20.2, lease time 86400), resolv.conf search lab.local, nameserver 10.2.50.10과 10.2.50.11 | 통과 |

BR1-SW1-L3 DHCP POOL
![BR1-SW1-L3 DHCP POOL](images/Pasted%20image%2020261017184726.png)

BR1-SW1-L3 DHCP LEASE
![BR1-SW1-L3 DHCP LEASE](images/Pasted%20image%2020261017184828.png)

BR1-SW2-L3 DHCP POOL
![BR1-SW2-L3 DHCP POOL](images/Pasted%20image%2020261017184913.png)

BR1-SW2-L3 DHCP LEASE
![BR1-SW2-L3 DHCP LEASE](images/Pasted%20image%2020261017184938.png)

BR1-PC1
![BR1-PC1](images/Pasted%20image%2020261017185105.png)

BR1-PC2
![BR1-PC2](images/Pasted%20image%2020261017185144.png)