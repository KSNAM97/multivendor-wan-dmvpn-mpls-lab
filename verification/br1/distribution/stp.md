# STP(MST) 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 3절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR1-SW1-L3 | `show spanning-tree mst 1` | 루트(priority 4096), region br1, instance 1 VLAN 10,100 | - | 미확인 |
| BR1-SW2-L3 | `show spanning-tree mst 1` | 보조 루트(8192), 루트는 SW1 | - | 미확인 |
