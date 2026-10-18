# QoS 검증 (HQ / core)

- Jira: KAN-25
- 대상 장비: HQ-R1, HQ-R2
- 기준 문서: [qos-design.md](../../../docs/qos-design.md), [cisco-router.md](../../../docs/config-guide/cisco-router.md) 3.7절
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| HQ-R1, HQ-R2 | `show policy-map` | 정책 `pm-lan-out`, `pm-lan-out-child` 정의, 클래스 `cm-control`, `cm-business`, `class-default` | - | 미확인 |
| HQ-R1, HQ-R2 | `show policy-map interface gi0/0`, `gi1/0` | `pm-lan-out` 출력 방향 적용, 클래스 3개 표시 | - | 미확인 |
| 호스트 | `nslookup` 후 `show policy-map interface` | `cm-business` 매치 패킷 증가 | - | 미확인 |
| 호스트 | 큰 트래픽(`ping -s 1400`) 후 `show policy-map interface` | `class-default`에 drop 발생, `cm-business` 보장 | - | 미확인 |
| HQ-R1, HQ-R2 | `show policy-map interface gi2/0`(해당 시) | 정책 미적용(제외 대상) | - | 미확인 |
