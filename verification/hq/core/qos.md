# QoS 검증 (HQ / core)

- Jira: KAN-25
- 대상 장비: HQ-R1, HQ-R2
- 기준 문서: [network-project-final.md](../../../docs/network-project-final.md) 5절, [cisco-router.md](../../../docs/config-guide/cisco-router.md) 3.7절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| HQ-R1, HQ-R2 | `show policy-map interface gi0/0` | `pm-lan-out` 적용, class `cm-control`, `cm-business`, default 카운트 | - | 미확인 |
| HQ-R1, HQ-R2 | `show policy-map` | 정책 `pm-lan-out`, `pm-lan-out-child` 정의 | - | 미확인 |
| 호스트 | `DNS(53) 트래픽 발생 후 정책 카운트` | `cm-business` 분류 카운트 증가 | - | 미확인 |
