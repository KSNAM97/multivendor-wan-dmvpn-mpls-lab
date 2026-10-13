# DNS 서버 검증 (DC / host)

- Jira: KAN-24
- 대상 장비: DC-DNS-1, DC-DNS-2
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 2절
- 검증일: 2026-10-13

| 장비       | 명령                                               | 기대 결과        | 실제 결과                                                                              | 판정  |
| -------- | ------------------------------------------------ | ------------ | ---------------------------------------------------------------------------------- | --- |
| DC-DNS-1 | `nslookup dc-r1.lab.local 10.2.50.10`            | 10.10.10.1   | 10.2.50.10:53 수신(`netstat -lun`), dnsmasq running, `dc-r1.lab.local` 응답 10.10.10.1 | 통과  |
| DC-DNS-2 | `nslookup dc-r1.lab.local 10.2.50.11`            | 10.10.10.1   | 10.2.50.11:53 수신(`netstat -lun`), dnsmasq running, `dc-r1.lab.local` 응답 10.10.10.1 | 통과  |
| 호스트 | `nslookup dc-r1.lab.local 10.2.50.11`(주 서버 중지 후) | 백업 서버가 같은 응답 | DC-DNS-1 중지 상태에서 10.2.50.10 조회는 connection timed out, 10.2.50.11 조회는 dc-r1.lab.local 10.10.10.1 응답. DC-DNS-2 콘솔 기준이며 호스트 resolv.conf 자동 전환은 2주차에 추가 확인 | 통과 |
| DC-DNS-1 | dnsmasq 로그(`log-queries`) | 질의 기록 | /var/log/dnsmasq.log에 read /etc/hosts.lab - 31 addresses, query[A] dc-r1.lab.local from 10.2.50.10, /etc/hosts.lab dc-r1.lab.local is 10.10.10.1 기록. 서버 자체 조회 기준이며 호스트 조회 로그는 2주차에 추가 | 통과 |

서버 콘솔 출력(텍스트)으로 확인. 서버 자체 조회까지이며 사이트 간 조회(HQ, BR 호스트)는 DMVPN 이후.

DC-DNS-1 구동확인
![DNS](images/Pasted%20image%2020261013205309.png)

DC-DNS-2 구동확인
![DNS](images/Pasted%20image%2020261013205338.png)

DC-DNS-1 DOWN시 DC-DNS-2 정상동작 확인
![DNS](images/Pasted%20image%2020261013205826.png)

DC-DNS-1 LOG
![DC-DNS-1 LOG](images/Pasted%20image%2020261013210246.png)
