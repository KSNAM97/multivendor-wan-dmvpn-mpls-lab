# MLAG 검증 (DC / distribution)

- Jira: -
- 대상 장비: DC-SW3-L3, DC-SW4-L3, DC-SW1-L2, DC-SW2-L2
- 기준 문서: [mlag-design.md](../../../docs/mlag-design.md)
- 검증일: -

| 장비 | 명령 | 기대 결과 | 실제 결과 | 판정 |
|---|---|---|---|---|
| DC-SW3-L3, DC-SW4-L3 | `show mlag` | State Active, Negotiation Connected, peer-link `Po1` Up | - | 미확인 |
| DC-SW3-L3, DC-SW4-L3 | `show mlag detail` | Config sanity consistent | - | 미확인 |
| DC-SW3-L3, DC-SW4-L3 | `show port-channel summary` | `Po10`, `Po20` Up(MLAG 포트채널) | - | 미확인 |
| DC-SW1-L2, DC-SW2-L2 | `show port-channel summary` | 업링크 Et1, Et2가 `Po10`, `Po20`로 묶여 Up | - | 미확인 |
| DC-SW1-L2, DC-SW2-L2 | `show lacp neighbor` | 두 파트너(SW3, SW4)와 LACP 협상 | - | 미확인 |
| DC-DNS-1 | `ping 10.2.50.1` 또는 `nslookup` | MLAG 적용 후에도 게이트웨이와 DNS 응답 유지 | - | 미확인 |
| DC-SW3-L3 | 멤버 포트 또는 장비 다운 후 확인 | 다른 쪽으로 트래픽 유지, 복구 후 정상화 | - | 미확인 |

cEOS의 MLAG 지원 여부는 `show mlag`로 먼저 확인하고, 미지원이면 판정란에 한계를 기록.
