# STP(MST) 검증 (HQ / distribution)

- Jira: KAN-9
- 대상 장비: HQ-SW-2, HQ-SW-3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 3절
- 검증일: 2026-10-11

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| HQ-SW-2 | `show spanning-tree mst 1` | 루트(priority 4096), Po1·Po3 designated, `Bound(RSTP)` 없음 | Bridge `0c65.58a4` priority 4097, "this switch for MST1", Po1 Desg FWD, Po3 Desg FWD | 통과 |
| HQ-SW-3 | `show spanning-tree mst 1` | 보조 루트(8192), Po3 Root FWD, Po2 Desg FWD | Bridge `0c9e.b2e0` priority 8193, Root `0c65.58a4` 4097(HQ-SW-2), Po3 Root FWD, Po2 Desg FWD | 통과 |


HQ-SW-2 MSTP
![HQ-SW-2 MSTP](images/Pasted%20image%2020261011235155.png)

HQ-SW-3 MSTP
![HQ-SW-3 MSTP](images/Pasted%20image%2020261011235205.png)