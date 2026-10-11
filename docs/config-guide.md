# 장비 설정 가이드

`configs/<사이트>/<장비명>.cfg`를 작성할 때의 순서, 공통 규칙, 장비 종류별 설정 틀을 정리한 문서. 값은 각 설계 문서의 표를 그대로 사용하며 아래 예시는 형식 확인용.

## 1. 설계 문서 대응

| 항목 | 문서 |
|---|---|
| 포트·연결 | `docs/port-mapping.md` |
| 인터페이스 IP | `docs/ip-mapping.md` |
| OSPF | `docs/ospf-design.md` |
| BGP(ISP) | `docs/bgp-design.md` |
| VLAN, HSRP/VARP, STP, EtherChannel | `docs/vlan-design.md` |
| DMVPN, IPsec | `docs/dmvpn-design.md` |
| DHCP, DNS | `docs/dhcp-dns-design.md` |

## 2. 작성 순서

| 단계 | 일정 | 대상 | 작업 | 기준 문서 |
|---|---|---|---|---|
| 1 | 1주 월~화 | 전 장비 | 호스트네임, Loopback0, 인터페이스 설명·IP, 콘솔 설정 | port-mapping, ip-mapping, ospf-design |
| 2 | 1주 수~목 | 스위치 | VLAN, 트렁크, EtherChannel, STP, SVI, HSRP/VARP | vlan-design |
| 3 | 1주 금 | 사이트 장비 | 사이트 내부 OSPF, 사용자 VLAN DHCP | ospf-design, dhcp-dns-design |
| 4 | 2주 월~화 | ISP 장비, 사이트 ISP 연결 라우터 | eBGP, PE 정적 경로·광고, 정적 기본 경로, IP SLA/track | bgp-design, ospf-design 6절 |
| 5 | 2주 수 | PrivI-net, PI 연결 라우터 | 언더레이 OSPF(프로세스 2), PI ACL | ospf-design 5절, dmvpn-design 5절 |
| 6 | 2주 목 | DC-R2, HQ-R2, BR1-R1, BR2-R2 | IPsec, Tunnel0, NHRP, 터널 OSPF | dmvpn-design |
| 7 | 2주 금 | DC-DNS-1, DC-DNS-2 | dnsmasq, 레코드 | dhcp-dns-design |
| 8 | 3주 월~화 | 라우터, DC 게이트웨이 | QoS, 서버 VLAN ACL | 아래 3.7절, dhcp-dns-design 5절 |

## 3. Cisco 7200 라우터

대상: `HQ-R*`, `DC-R*`, `BR*-R*`, `ISP-*`, `PrivI-net`. 인터페이스 이름은 `GigabitEthernet<슬롯>/0`.

### 3.1 기본

```
hostname HQ-R2
enable secret admin1234
no ip domain-lookup
service timestamps debug datetime msec
service timestamps log datetime msec
!
line con 0
 logging synchronous
 exec-timeout 0 0
line vty 0 4
 password admin1234
 login
!
interface Loopback0
 ip address 11.11.11.2 255.255.255.255
!
interface GigabitEthernet0/0
 description to HQ-SW-2 Gi1/0
 ip address 10.1.0.5 255.255.255.252
 no shutdown
```

- 콘솔에서 `enable`, `configure terminal`로 설정 모드에 진입한 뒤 위 내용을 붙여 넣음(진입 명령은 파일에 기재하지 않음).
- 접속 설정(`enable secret`, `line vty`)은 모든 Cisco 장비에 같은 형식으로 적용. Arista는 `username admin secret admin1234`.
- 실습 환경이므로 접속 비밀번호는 `admin1234`로 통일하고 컨피그와 문서에 그대로 기재(7절).
- 모든 인터페이스에 `description`(연결 상대와 포트)을 입력하고 `no shutdown`을 명시.

### 3.2 OSPF(사이트 프로세스)

```
router ospf 11
 router-id 11.11.11.2
 passive-interface Loopback0
!
interface Loopback0
 ip ospf 11 area 1
interface GigabitEthernet0/0
 ip ospf 11 area 1
 ip ospf network point-to-point
```

- 프로세스, Area, Router ID는 `docs/ospf-design.md` 2~3절의 표 값을 사용. 터널 인터페이스는 3.5절 참조.

### 3.3 ISP 연결(정적 기본 경로, IP SLA, 기본 경로 광고)

```
ip sla 1
 icmp-echo 100.2.12.2 source-interface GigabitEthernet3/0
 frequency 5
ip sla schedule 1 life forever start-time now
track 1 ip sla 1 reachability
ip route 0.0.0.0 0.0.0.0 100.2.12.2 track 1
!
router ospf 11
 default-information originate metric 200 metric-type 1
```

- 예시는 `HQ-R2`(ISP-2 연결). ISP-1 연결 라우터는 metric 100. 대상 장비와 값은 `docs/ospf-design.md` 6절 표.

### 3.4 ISP 라우터 eBGP

```
router bgp 65111
 bgp router-id 111.111.111.111
 bgp log-neighbor-changes
 neighbor 100.1.0.2 remote-as 65112
 neighbor 100.1.0.2 timers 3 9
 network 10.1.0.0 mask 255.255.0.0
ip route 10.1.0.0 255.255.0.0 100.1.11.1
```

- 예시는 `ISP-1-PE-A`. 세션, AS, 광고 prefix는 `docs/bgp-design.md` 3~4절 표 값을 사용. P1, P2는 `network` 구문 없음.

### 3.5 DMVPN(Hub와 Spoke)

```
crypto isakmp policy 10
 encryption aes 256
 hash sha
 authentication pre-share
 group 5
 lifetime 86400
crypto isakmp key DMVPN-HUB address 0.0.0.0 0.0.0.0
crypto isakmp keepalive 10 3
crypto ipsec transform-set TS-AES256 esp-aes 256 esp-sha-hmac
 mode tunnel
crypto ipsec profile DMVPN-PROFILE
 set transform-set TS-AES256
 set pfs group5
 set security-association lifetime seconds 3600
!
interface Tunnel0
 ip address 172.16.0.13 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet4/0
 tunnel mode gre multipoint
 tunnel key 100
 ip nhrp network-id 100
 ip nhrp holdtime 600
 ip nhrp authentication NHRPKEY1
 ip nhrp nhs 172.16.0.1
 ip nhrp map 172.16.0.1 203.0.113.5
 ip nhrp map multicast 203.0.113.5
 ip nhrp shortcut
 ip ospf 11 area 0
 ip ospf network broadcast
 ip ospf priority 0
 tunnel protection ipsec profile DMVPN-PROFILE shared
```

- 예시는 Spoke `HQ-R2`. Hub(`DC-R2`)는 `ip nhrp map multicast dynamic`, `ip nhrp redirect`, `ip ospf priority 255`를 사용하고 NHS·map·shortcut 줄 제외.

### 3.6 Private Internet 언더레이와 ACL

```
router ospf 2
 router-id 11.11.11.2
interface GigabitEthernet4/0
 ip ospf 2 area 100
 ip ospf network point-to-point
!
ip access-list extended PI-WHITELIST
 permit ospf any any
 permit gre 203.0.113.0 0.0.0.255 203.0.113.0 0.0.0.255
 permit esp 203.0.113.0 0.0.0.255 203.0.113.0 0.0.0.255
 permit udp 203.0.113.0 0.0.0.255 203.0.113.0 0.0.0.255 eq isakmp
 permit icmp any any
 deny ip any any log
```

- ACL은 `PrivI-net`의 5개 링크 인터페이스에 적용(`ip access-group PI-WHITELIST in`). `ICMP` 허용 줄은 임시이며 검증 후 제거.
- 사이트 라우터는 사이트 프로세스와 같은 Router ID를 프로세스 2에도 사용. `PrivI-net`의 Router ID는 `2.2.2.2`.

### 3.7 QoS

```
class-map match-any CM-CONTROL
 match dscp cs6
class-map match-any CM-BUSINESS
 match dscp af31
!
policy-map PM-LAN-OUT-CHILD
 class CM-CONTROL
  bandwidth percent 10
 class CM-BUSINESS
  bandwidth percent 30
 class class-default
  fair-queue
policy-map PM-LAN-OUT
 class class-default
  shape average 10000000
  service-policy PM-LAN-OUT-CHILD
!
interface GigabitEthernet0/0
 service-policy output PM-LAN-OUT
```

- 적용 대상은 사이트 스위치와 연결된 라우터 인터페이스(`Gi0/0`, `Gi1/0`) 20포트(`docs/network-project-final.md` 5절). 제외: 라우터 간 링크, ISP 방향, PI 방향, Tunnel0.
- `shape average` 값은 병목 검증용이며 DNS(53) 트래픽은 `match access-group`으로 `CM-BUSINESS`에 분류하고 `set dscp af31`로 표시. 스위치 QoS는 적용하지 않음.

## 4. Cisco 스위치(vIOS L3·L2)

대상: `HQ-SW-*`, `BR1-SW*-L3`, `BR2-SW1-L3`, `BR2-SW2-L3`(L3), `BR2-SW3-L2`(L2). 인터페이스 이름은 `GigabitEthernet<슬롯>/<번호>`.

### 4.1 L2 기능

```
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20 priority 4096
vlan 10
 name HQ-USER-A
vlan 20
 name HQ-USER-B
vlan 100
 name MGMT
vlan 999
 name NATIVE
!
interface range GigabitEthernet0/0 - 1
 channel-group 1 mode active
interface Port-channel1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,100
interface GigabitEthernet3/0
 switchport mode access
 switchport access vlan 10
```

- 채널 번호, 허용 VLAN, 포트는 `docs/vlan-design.md` 4~6절 표 값을 사용. 루트 장비에만 STP priority 4096, Standby 장비는 8192.

### 4.2 L3 기능(SVI, HSRP, 라우티드 포트)

```
ip routing
interface Vlan10
 ip address 10.1.10.2 255.255.255.0
 standby version 2
 standby 10 ip 10.1.10.1
 standby 10 priority 110
 standby 10 preempt
!
interface GigabitEthernet1/0
 no switchport
 ip address 10.1.0.2 255.255.255.252
```

- 예시는 `HQ-SW-2`(Active). `HQ-SW-3`은 SVI 주소 `.3`, priority 100. 라우티드 포트의 `ip ospf` 설정은 3.2절과 같은 형식.
- 사용자 VLAN의 SVI는 `passive-interface`로 설정하고 사이트 OSPF Area에 포함. DHCP 풀은 `docs/dhcp-dns-design.md` 3절.
- 이미지(`vios_l2`)에서 `no switchport`와 `ip routing` 동작은 첫 장비에 적용해 확인 후 나머지 장비에 반복.

### 4.3 L2 스위치 예시(BR2-SW3-L2)

- L3 기능 없이 트렁크 2포트(L3 스위치 방향)와 호스트 액세스 2포트만 설정.

```
hostname BR2-SW3-L2
spanning-tree mode rapid-pvst
vlan 10
 name BR2-USER
vlan 100
 name MGMT
vlan 999
 name NATIVE
!
interface GigabitEthernet0/0
 description to BR2-SW1-L3 Gi2/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,100
interface GigabitEthernet0/1
 description to BR2-SW2-L3 Gi2/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,100
interface GigabitEthernet3/0
 description to BR2-PC1 eth0
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
interface GigabitEthernet3/1
 description to BR2-PC2 eth0
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
interface Vlan100
 ip address 10.4.100.23 255.255.255.0
```

## 5. Arista cEOS(DC)

대상: `DC-SW1-L3` ~ `DC-SW4-L3`, `DC-SW1-L2`, `DC-SW2-L2`. 인터페이스 이름은 `Ethernet<번호>`, 접두 3칸 들여쓰기.

```
hostname DC-SW3-L3
spanning-tree mode rstp
spanning-tree priority 4096
ip routing
ip virtual-router mac-address 00:1c:73:00:00:99
!
vlan 50
   name SERVER
vlan 100
   name MGMT
vlan 999
   name NATIVE
!
interface Ethernet5-6
   channel-group 1 mode active
interface Port-Channel1
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,100
interface Vlan50
   ip address 10.2.50.2/24
   ip virtual-router address 10.2.50.1
interface Ethernet3
   no switchport
   ip address 10.2.0.38/30
   ip ospf network point-to-point
!
router ospf 10
   router-id 10.10.10.13
   passive-interface Vlan50
   network 10.2.0.36/30 area 0.0.0.0
   network 10.2.50.0/24 area 0.0.0.0
```

- 예시는 `DC-SW3-L3`. `DC-SW4-L3`은 SVI 주소 `.3`, STP priority 8192. `DC-SW1-L3`, `DC-SW2-L3`은 VLAN 설정 없이 라우티드 포트와 OSPF만 사용.
### 5.1 DC L2 스위치(DC-SW1-L2, DC-SW2-L2)

- `ip routing`, OSPF 없이 트렁크(Et1, Et2)와 서버 액세스(Et12, VLAN 50)만 설정. EOS의 포트는 기본이 스위치포트이므로 `no switchport` 불필요.

```
hostname DC-SW1-L2
spanning-tree mode rstp
!
vlan 50
   name SERVER
vlan 100
   name MGMT
vlan 999
   name NATIVE
!
interface Ethernet1
   description to DC-SW3-L3 Et7
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,100
interface Ethernet2
   description to DC-SW4-L3 Et7
   switchport mode trunk
   switchport trunk native vlan 999
   switchport trunk allowed vlan 50,100
interface Ethernet12
   description to DC-DNS-1 eth0
   switchport mode access
   switchport access vlan 50
   spanning-tree portfast
interface Vlan100
   ip address 10.2.100.21/24
```

- `DC-SW2-L2`는 호스트네임 `DC-SW2-L2`, 업링크 설명 `to DC-SW3-L3 Et8`(Et1), `to DC-SW4-L3 Et8`(Et2), 서버 포트 설명 `to DC-DNS-2 eth0`, MGMT 주소 `10.2.100.22/24`로 변경.

## 6. 알파인 노드

- `DC-DNS-1`, `DC-DNS-2`: `docs/dhcp-dns-design.md` 2절의 `interfaces`, `dnsmasq.conf`, `hosts.lab` 사용.
- 호스트(`HQ-PC*`, `BR*-PC*`): DHCP 클라이언트(`docs/dhcp-dns-design.md` 4절). 별도 `.cfg` 파일 없음.

## 7. 저장과 백업

- 변경 전 장비에서 `copy running-config flash:<장비명>-backup-<날짜>.cfg`로 백업.
- 변경 후 `copy running-config startup-config`(EOS는 `write`)로 저장하고, 확인한 컨피그를 `configs/<사이트>/<장비명>.cfg`에 반영.
- 실습 환경이므로 접속 비밀번호(`admin1234`)와 DMVPN 키(ISAKMP `DMVPN-HUB`, NHRP `NHRPKEY1`)를 컨피그와 문서에 그대로 기재.

## 8. 설정 후 확인

| 항목 | Cisco | Arista |
|---|---|---|
| 인터페이스 | `show ip interface brief` | `show ip interface brief` |
| OSPF | `show ip ospf neighbor`, `show ip route ospf` | `show ip ospf neighbor`, `show ip route ospf` |
| BGP | `show ip bgp summary` | - |
| HSRP/VARP | `show standby brief` | `show ip virtual-router` |
| 채널, 트렁크 | `show etherchannel summary`, `show interfaces trunk` | `show port-channel summary`, `show interfaces trunk` |
| STP | `show spanning-tree` | `show spanning-tree` |
| DMVPN | `show dmvpn`, `show ip nhrp`, `show crypto ipsec sa` | - |

