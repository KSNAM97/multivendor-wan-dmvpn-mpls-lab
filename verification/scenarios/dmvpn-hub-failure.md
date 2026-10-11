# DMVPN Hub 장애 시 동작

- Jira: KAN-30
- 대상 장비: DC-R2, HQ-R2, BR1-R1, BR2-R2
- 기준 문서: [dmvpn-design.md](../../docs/dmvpn-design.md)
- 검증일: -

| 순서 | 동작 | 기대 결과 | 측정값 | 판정 |
|---|---|---|---|---|
| 1 | Hub(DC-R2) 중지 또는 Tunnel0 `shutdown` | Spoke의 NHRP 등록 끊김 | - | 미확인 |
| 2 | Spoke 간 통신 | Phase 3로 이미 만들어진 shortcut은 유지, 새 Spoke 간 연결은 불가 | - | 미확인 |
| 3 | Private Internet 경로 | Hub 장애와 무관하게 언더레이는 유지 | - | 미확인 |
| 4 | Hub 복구 | NHRP 재등록, OSPF 이웃 FULL 복귀 | - | 미확인 |

Hub가 하나뿐인 구조의 한계를 확인하는 시나리오.
