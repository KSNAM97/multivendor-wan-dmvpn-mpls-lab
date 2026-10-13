# DHCP 검증 (BR1 / distribution)

- Jira: KAN-14
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 3절
- 검증일: 2026-10-13

| 장비                     | 명령                                                  | 기대 결과                                            | 실제 결과 | 판정  |
| ---------------------- | --------------------------------------------------- | ------------------------------------------------ | ----- | --- |
| BR1-SW1-L3, BR1-SW2-L3 | `show ip dhcp pool` | 풀 범위 Active `.100`~`.163`, Standby `.164`~`.227` | 두 스위치 모두 풀 br1-public, br1-private, Total 254, Excluded 190. BR1-SW1-L3(Active)는 임대 시작 지수 .101(Active 범위 .100~), BR1-SW2-L3(Standby)는 지수 .165(Standby 범위 .164~) | 통과 |
| BR1-SW1-L3, BR1-SW2-L3 | `show ip dhcp binding` | 호스트에 풀 범위 주소 임대, 게이트웨이 VIP, DNS 10.2.50.10 / .11 | BR1-SW1-L3에 10.3.10.100(Vlan10), 10.3.20.100(Vlan20) 임대, 클라이언트 ID가 호스트 MAC과 일치, 임대 만료 24시간 후. BR1-SW2-L3는 먼저 응답한 SW1이 할당해 바인딩 없음 | 통과 |
| 호스트 | `udhcpc -i eth0`, `ip addr`, `cat /etc/resolv.conf` | 풀 범위 주소와 `resolv.conf` DNS 두 대 | BR1-PC1 10.3.10.100(서버 10.3.10.2), BR1-PC2 10.3.20.100(서버 10.3.20.2), lease time 86400, resolv.conf search lab.local, nameserver 10.2.50.10과 10.2.50.11 | 통과 |
BR1-SW1-L3 DHCP POOL
![BR1](images/Pasted%20image%2020261013202525.png)

BR1-SW1-L3 DHCP BINDING
![BR1](images/Pasted%20image%2020261013202536.png)

BR1-SW2-L3 DHCP POOL
![BR1](images/Pasted%20image%2020261013202611.png)

BR1-SW2-L3 DHCP BINDING
![BR1](images/Pasted%20image%2020261013202620.png)

BR1-PC1
![BR1](images/Pasted%20image%2020261013202422.png)

BR1-PC2
![BR1](images/Pasted%20image%2020261013202444.png)