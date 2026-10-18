# EtherChannel 멤버 다운 실측

- Jira: KAN-28
- 대상 장비: HQ Po1~Po3, DC Po1, BR1 Po1, BR2 Po1
- 기준 문서: [vlan-design.md](../../docs/vlan-design.md) 4절
- 검증일: -

| 순서 | 동작 | 기대 결과 | 측정값 | 판정 |
|---|---|---|---|---|
| 1 | 트래픽(`ping`) 연속 실행 | 응답 정상 | - | 미확인 |
| 2 | 채널 멤버 포트 하나 `shutdown` | 채널 유지(남은 멤버 `P`), 패킷 손실 최소 | - | 미확인 |
| 3 | 나머지 멤버도 `shutdown` | 채널 다운, STP 재계산으로 대체 경로 사용 | - | 미확인 |
| 4 | 멤버 복구 | 채널 정상 복귀 | - | 미확인 |

- DC MLAG 포트(`DC-SW3-L3` Et7 다운)에서의 결과는 [dc/distribution/mlag.md](../dc/distribution/mlag.md) 참조(연속 ping 1개 손실, 복구 후 Active-full 2).
