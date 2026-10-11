# VLAN 설계

VLAN, 게이트웨이 이중화, EtherChannel, 트렁크, STP, DHCP 배치를 정리한 문서. 포트는 `docs/port-mapping.md`, 서브넷과 DHCP 풀은 `docs/ip-mapping.md` 기준.

## 1. VLAN 정의

| 사이트 | VLAN | 이름 | 서브넷 | 태그 | 게이트웨이 이중화 |
|---|---|---|---|---|---|
| HQ | 10 | HQ-USER-A | 10.1.10.0/24 | `vlan-hq-user-a` | HSRP |
| HQ | 20 | HQ-USER-B | 10.1.20.0/24 | `vlan-hq-user-b` | HSRP |
| DC | 50 | SERVER | 10.2.50.0/24 | `vlan-dc-server` | VARP |
| BR1 | 10 | BR1-USER | 10.3.10.0/24 | `vlan-br1-user` | HSRP |
| BR2 | 10 | BR2-USER | 10.4.10.0/24 | `vlan-br2-user` | HSRP |
| 전 사이트 | 100 | MGMT | 10.<사이트>.100.0/24 | `vlan-mgmt` | - |
| 전 사이트 | 999 | NATIVE | 없음 | `vlan-native` | - |

- 모든 트렁크의 native VLAN은 999.
- MGMT VLAN 100은 스위치마다 SVI를 두며 주소는 `10.<사이트>.100.<장비 번호>/24`. 장비 번호는 L3·L2 스위치가 이름의 번호에 10을 더한 값(예: `HQ-SW-2` → `10.1.100.12`).

## 2. 게이트웨이 이중화

- HSRP는 version 2, 그룹 번호는 VLAN 번호와 동일, `preempt` 사용. 우선순위는 Active 110, Standby 100.
- DC 서버 VLAN은 Arista VARP로 두 스위치가 같은 가상 MAC과 VIP를 공유(우선순위 개념 없음).

| 사이트 | VLAN | 방식 | Active / 1번 장비 | Standby / 2번 장비 | VIP | 서브넷 |
|---|---|---|---|---|---|---|
| HQ | 10 | HSRP | HQ-SW-2 `10.1.10.2` (110) | HQ-SW-3 `10.1.10.3` (100) | 10.1.10.1 | 10.1.10.0/24 |
| HQ | 20 | HSRP | HQ-SW-2 `10.1.20.2` (110) | HQ-SW-3 `10.1.20.3` (100) | 10.1.20.1 | 10.1.20.0/24 |
| DC | 50 | VARP | DC-SW3-L3 `10.2.50.2` | DC-SW4-L3 `10.2.50.3` | 10.2.50.1 | 10.2.50.0/24 |
| BR1 | 10 | HSRP | BR1-SW1-L3 `10.3.10.2` (110) | BR1-SW2-L3 `10.3.10.3` (100) | 10.3.10.1 | 10.3.10.0/24 |
| BR2 | 10 | HSRP | BR2-SW1-L3 `10.4.10.2` (110) | BR2-SW2-L3 `10.4.10.3` (100) | 10.4.10.1 | 10.4.10.0/24 |

## 3. STP

- Cisco 스위치(HQ, BR1, BR2): `spanning-tree mode rapid-pvst`. Arista 스위치(DC): `spanning-tree mode rstp`.
- 루트 브리지는 게이트웨이 Active 장비로 고정(해당 VLAN priority 4096, Standby는 8192).

| 사이트 | 루트 | 보조 루트 |
|---|---|---|
| HQ | HQ-SW-2 | HQ-SW-3 |
| DC | DC-SW3-L3 | DC-SW4-L3 |
| BR1 | BR1-SW1-L3 | BR1-SW2-L3 |
| BR2 | BR2-SW1-L3 | BR2-SW2-L3 |

## 4. EtherChannel

- 모든 채널은 LACP(`channel-group <번호> mode active`) 사용.

| 사이트 | Po | 장비 A | A 포트 | 장비 B | B 포트 | 허용 VLAN |
|---|---|---|---|---|---|---|
| HQ | Po1 | HQ-SW-1 | Gi1/0, Gi1/1 | HQ-SW-2 | Gi0/0, Gi0/1 | 10,20,100 |
| HQ | Po2 | HQ-SW-1 | Gi1/2, Gi1/3 | HQ-SW-3 | Gi0/0, Gi0/1 | 10,20,100 |
| HQ | Po3 | HQ-SW-2 | Gi0/2, Gi0/3 | HQ-SW-3 | Gi0/2, Gi0/3 | 10,20,100 |
| DC | Po1 | DC-SW3-L3 | Et5, Et6 | DC-SW4-L3 | Et5, Et6 | 50,100 |
| BR1 | Po1 | BR1-SW1-L3 | Gi1/0, Gi1/1 | BR1-SW2-L3 | Gi1/0, Gi1/1 | 10,100 |
| BR2 | Po1 | BR2-SW1-L3 | Gi1/0, Gi1/1 | BR2-SW2-L3 | Gi1/0, Gi1/1 | 10,100 |

## 5. 트렁크

- 허용 VLAN은 사이트별 값(HQ 10, 20, 100 / DC 50, 100 / BR1·BR2 10, 100)만 통과, native 999.

| 사이트 | 장비 A | A 포트 | 장비 B | B 포트 | 허용 VLAN |
|---|---|---|---|---|---|
| DC | DC-SW3-L3 | Et7 | DC-SW1-L2 | Et1 | 50,100 |
| DC | DC-SW3-L3 | Et8 | DC-SW2-L2 | Et1 | 50,100 |
| DC | DC-SW4-L3 | Et7 | DC-SW1-L2 | Et2 | 50,100 |
| DC | DC-SW4-L3 | Et8 | DC-SW2-L2 | Et2 | 50,100 |
| BR1 | BR1-SW1-L3 | Gi2/0 | BR1-SW3-L3 | Gi0/0 | 10,100 |
| BR1 | BR1-SW2-L3 | Gi2/0 | BR1-SW3-L3 | Gi0/1 | 10,100 |
| BR2 | BR2-SW1-L3 | Gi2/0 | BR2-SW3-L2 | Gi0/0 | 10,100 |
| BR2 | BR2-SW2-L3 | Gi2/0 | BR2-SW3-L2 | Gi0/1 | 10,100 |

## 6. 액세스 포트

| 장비 A | A 포트 | 호스트 장비 | 포트 | VLAN |
|---|---|---|---|---|
| HQ-SW-1 | Gi3/0 | HQ-PC1 | eth0 | 10 |
| HQ-SW-1 | Gi3/1 | HQ-PC2 | eth0 | 20 |
| DC-SW1-L2 | Et12 | DC-DNS-1 | eth0 | 50 |
| DC-SW2-L2 | Et12 | DC-DNS-2 | eth0 | 50 |
| BR1-SW3-L3 | Gi3/0 | BR1-PC1 | eth0 | 10 |
| BR1-SW3-L3 | Gi3/1 | BR1-PC2 | eth0 | 10 |
| BR2-SW3-L2 | Gi3/0 | BR2-PC1 | eth0 | 10 |
| BR2-SW3-L2 | Gi3/1 | BR2-PC2 | eth0 | 10 |

## 7. DHCP 배치

- DHCP는 사용자 VLAN의 게이트웨이 장비 두 대가 함께 제공하고, 풀 범위를 나눠 중복 할당을 방지. 서버 VLAN은 DHCP 없이 고정 주소 사용.
- 풀 전체는 `.100` – `.227`(128개). Active 장비가 앞 64개, Standby 장비가 뒤 64개 담당. `default-router`는 VIP, DNS는 `10.2.50.10`, `10.2.50.11`.

| 사이트 | VLAN | Active 장비 풀 | Standby 장비 풀 |
|---|---|---|---|
| HQ | 10 | HQ-SW-2 `10.1.10.100` – `10.1.10.163` | HQ-SW-3 `10.1.10.164` – `10.1.10.227` |
| HQ | 20 | HQ-SW-2 `10.1.20.100` – `10.1.20.163` | HQ-SW-3 `10.1.20.164` – `10.1.20.227` |
| BR1 | 10 | BR1-SW1-L3 `10.3.10.100` – `10.3.10.163` | BR1-SW2-L3 `10.3.10.164` – `10.3.10.227` |
| BR2 | 10 | BR2-SW1-L3 `10.4.10.100` – `10.4.10.163` | BR2-SW2-L3 `10.4.10.164` – `10.4.10.227` |

