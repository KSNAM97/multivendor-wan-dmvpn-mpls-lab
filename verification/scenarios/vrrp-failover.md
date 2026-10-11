# VRRP 장애 전환 실측

- Jira: KAN-27
- 대상 장비: HQ-SW-2/3, DC-SW3-L3/SW4-L3, BR1-SW1-L3/SW2-L3, BR2-SW1-L3/SW2-L3
- 기준 문서: [vrrp-mstp-design.md](../../docs/vrrp-mstp-design.md) 2절
- 검증일: -

| 순서 | 동작 | 기대 결과 | 측정값 | 판정 |
|---|---|---|---|---|
| 1 | 호스트에서 VIP로 연속 `ping` 시작 | 응답 정상 | - | 미확인 |
| 2 | Active 장비의 사용자 VLAN SVI `shutdown` | Backup이 Master로 전환, 호스트 `ping` 중단 시간 측정 | - | 미확인 |
| 3 | SVI `no shutdown`(Arista는 `no shutdown`) | `preempt`로 Active가 Master 복귀, 중단 시간 측정 | - | 미확인 |
| 4 | 사이트별 반복 | 모든 사이트에서 동일 결과 | - | 미확인 |

중단 시간은 측정값 열에 초 단위로 기록하고 면접용 근거로 사용.
