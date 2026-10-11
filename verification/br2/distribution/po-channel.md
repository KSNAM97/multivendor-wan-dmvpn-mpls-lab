# EtherChannel 검증 (BR2 / distribution)

- Jira: KAN-13
- 대상 장비: BR2-SW1-L3, BR2-SW2-L3
- 기준 문서: [vlan-design.md](../../../docs/vlan-design.md) 1, 4~6절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-SW1-L3 | `show etherchannel summary` | Po1(Gi1/0, Gi1/1) LACP, 멤버 `(P)` | - | 미확인 |
| BR2-SW2-L3 | `show etherchannel summary` | Po1(Gi1/0, Gi1/1) LACP, 멤버 `(P)` | - | 미확인 |
