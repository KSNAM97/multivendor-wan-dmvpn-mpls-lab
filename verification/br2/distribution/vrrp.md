# VRRP 검증 (BR2 / distribution)

- Jira: KAN-13
- 대상 장비: BR2-SW1-L3, BR2-SW2-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 2절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| BR2-SW1-L3 | `show vrrp brief` | Vl10 그룹 10, Vl20 그룹 20, priority 110, Master, VIP 10.4.10.1, 10.4.20.1 | - | 미확인 |
| BR2-SW2-L3 | `show vrrp brief` | Vl10 그룹 10, Vl20 그룹 20, priority 90, Backup, Master addr 10.4.10.2, 10.4.20.2 | - | 미확인 |
