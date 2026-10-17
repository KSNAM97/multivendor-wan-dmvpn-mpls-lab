# VLAN, 트렁크 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: 2026-10-17
- Aruba CX 교체: BR1 스위치를 Aruba CX(ArubaOS-CX Virtual 10.07)로 교체하고 AOS-CX 명령 기준으로 재검증

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-SW1-L3 | `show interface trunk` | lag1과 BR1-SW3-L3 방향 `1/1/5` 트렁크, 허용 10,20,100(native 999는 허용 목록에 없음) | 1/1/5, lag1 Trunk VLANs 10,20,100. Native VLAN은 None으로 표시 | 통과 |
| BR1-SW2-L3 | `show interface trunk` | lag1과 BR1-SW3-L3 방향 `1/1/5` 트렁크, 허용 10,20,100(native 999는 허용 목록에 없음) | 1/1/5, lag1 Trunk VLANs 10,20,100. Native VLAN은 None으로 표시 | 통과 |
| BR1-SW1-L3, BR1-SW2-L3 | `show vlan` | VLAN 10 br1-public, 20 br1-private, 100 mgmt up | VLAN 10 br1-public, 20 br1-private, 100 mgmt 모두 up(1/1/5, lag1), 999 native와 1 DEFAULT_VLAN_1은 down(no_member_port). SW1, SW2 동일 | 통과 |

트렁크 링크: BR1-SW1-L3 1/1/5 ↔ BR1-SW3-L3 1/1/1, BR1-SW2-L3 1/1/5 ↔ BR1-SW3-L3 1/1/2

BR1-SW1-L3 TRUNK
![BR1-SW1-L3 TRUNK](images/Pasted%20image%2020261017185531.png)

BR1-SW1-L3 VLAN
![BR1-SW1-L3 VLAN](images/Pasted%20image%2020261017185522.png)

BR1-SW2-L3 TRUNK
![BR1-SW2-L3 TRUNK](images/Pasted%20image%2020261017185453.png)

BR1-SW2-L3 VLAN
![BR1-SW2-L3 VLAN](images/Pasted%20image%2020261017185505.png)