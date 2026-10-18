# 서버 VLAN ACL 검증 (DC / core)

- Jira: KAN-26
- 대상 장비: DC-R1, DC-R2, DC-R3, DC-R4 (DC-SW3-L3, DC-SW4-L3는 cEOS ACL 미지원 확인)
- 기준 문서: [dhcp-dns-design.md](../../../docs/dhcp-dns-design.md) 5절
- 검증일: 2026-10-18

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-SW3-L3 | `int vlan50`, `ip access-group dns-only out` | 적용(설계 초안) | `% Unavailable command (not supported on this hardware platform)`로 거부 | cEOS 미지원 확인 |
| DC-SW3-L3 | `int range et1-4`, `ip access-group dns-only in` | 적용(대체안) | 같은 메시지로 거부 | cEOS 미지원 확인, 적용 위치를 DC 라우터로 변경 |
| DC-R1, DC-R2, DC-R3, DC-R4 | `show ip access-lists dns-only` | DNS(UDP/TCP 53) 허용 4항목, 서버 VLAN deny(로그), `permit ip any any` | 4대 모두 10 `permit udp any host 10.2.50.10 eq domain`, 20 `permit tcp ...`, 30 `permit udp any host 10.2.50.11 eq domain`, 40 `permit tcp ...`, 50 `deny ip any 10.2.50.0 0.0.0.255 log`, 60 `permit ip any any` 구성 확인. 매치 카운트는 이번 캡처에 표시되지 않음 | 구성 통과 |
| DC-R1, DC-R2, DC-R3, DC-R4 | `show ip interface gi0/0 \| include access list` | 스위치 방향 인터페이스에 `dns-only` 출력 방향 적용 | 4대 모두 `Outgoing access list is dns-only`, `Inbound access list is not set` | 통과 |
| 호스트 | `nslookup dc-r1.lab.local 10.2.50.10` | 10.10.10.1 응답 | `Server: 10.2.50.10`, `Name: dc-r1.lab.local`, `Address: 10.10.10.1`(Non-authoritative answer) | 통과 |
| 호스트 | `nc -zv -w 2 10.2.50.10 22` | DNS 외 포트 차단 | `nc: 10.2.50.10 (10.2.50.10:22): Host is unreachable` | 통과 |
| 호스트 | `ping -c 2 10.2.50.10` | ICMP 차단(ACL 적용 후 정상) | 2 packets transmitted, 0 packets received, 100% packet loss | 통과 |
| 호스트 경로상 DC 라우터 | `show ip access-lists dns-only` (시험 후) | 허용 항목과 `deny` 항목 매치 카운트 증가 | - | 미확인 |

시험 후 라우터 ACL 매치 카운트와 `deny` 로그(`%SEC-6-IPACCESSLOGP`) 캡처는 보충 예정.

DC-SW3-L3 ip access-group out 거부
![DC-SW3-L3 ip access-group out 거부](images/acl-ceos-svi-out.png)

DC-SW3-L3 ip access-group in 거부
![DC-SW3-L3 ip access-group in 거부](images/acl-ceos-port-in.png)

DC-R1 dns-only
![DC-R1 dns-only](images/acl-dc-r1.png)

DC-R2 dns-only
![DC-R2 dns-only](images/acl-dc-r2.png)

DC-R3 dns-only
![DC-R3 dns-only](images/acl-dc-r3.png)

DC-R4 dns-only
![DC-R4 dns-only](images/acl-dc-r4.webp)

호스트 nslookup, ping, nc 시험
![호스트 nslookup, ping, nc 시험](images/acl-host-test.webp)
