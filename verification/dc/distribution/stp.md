# STP(MST) 검증 (DC / distribution)

- Jira: KAN-11
- 대상 장비: DC-SW3-L3, DC-SW4-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 3절
- 검증일: 2026-10-11

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-SW3-L3 | `show spanning-tree mst 1` | 루트(priority 4096), 모든 포트 designated | Bridge `0c06.2ecc.02b8` priority 4097, "this switch for MST1", Et7 Et8 Po1 designated forwarding | 통과 |
| DC-SW4-L3 | `show spanning-tree mst 1` | 보조 루트(8192), Po1 root | Bridge `0c0d.0b7c.425d` priority 8193, Root 4097(DC-SW3-L3), Po1 root FWD, Et7 Et8 designated FWD | 통과 |
DC-SW3-L3 MSTP
![DC-SW3-L3 MSTP](images/Pasted%20image%2020261011233709.png)

DC-SW4-L3 MSTP
![DC-SW4-L3 MSTP](images/Pasted%20image%2020261011233647.png)