# Cisco 라우터 설정 (7200)

> 설정 가이드 목차: [README.md](./README.md) | [Cisco 라우터](./cisco-router.md) | [Cisco 스위치](./cisco-switch.md) | [Arista 스위치](./arista-switch.md) | [알파인](./alpine.md)

대상: `HQ-R*`, `DC-R*`, `BR*-R*`, `ISP-*`, `Private-net`. 인터페이스 이름은 `gi<슬롯>/0`.

### 3.1 기본

```
! cisco
!
hostname hq-r2
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
int lo0
 ip addr 11.11.11.2 255.255.255.255
!
int gi0/0
 desc to hq-sw-2 gi1/0
 ip addr 10.1.0.5 255.255.255.252
 no sh
```

- 콘솔에서 `enable`, `configure terminal`로 설정 모드에 진입한 뒤 위 내용을 붙여 넣음(진입 명령은 파일에 기재하지 않음).
- 접속 설정(`enable secret`, `line vty`)은 모든 Cisco 장비에 같은 형식으로 적용. Arista는 `username admin secret admin1234`.
- 실습 환경이므로 접속 비밀번호는 `admin1234`로 통일하고 컨피그와 문서에 그대로 기재([README.md](./README.md) 7절).
- 물리 포트와 `po`(연결 상대와 포트)에는 `desc`를 입력하고 Cisco는 `no sh`를 명시. Arista는 포트가 기본 활성이므로 `no sh`를 입력하지 않음(EOS에서 `no sh`는 모호한 명령으로 거절됨). 가상 인터페이스(`lo0`)와 MGMT SVI(`vlan 100`, VLAN `name`으로 구분)는 `desc` 생략. 단, EtherChannel 멤버 포트는 `int range`로 묶어 범위에 `desc to <상대 장비> po<번호>`를 한 번만 입력(포트별 `desc`는 생략).

### 3.2 OSPF(사이트 프로세스)

```
router ospf 11
 router-id 11.11.11.2
 passive-interface lo0
!
int lo0
 ip ospf 11 area 1
int gi0/0
 ip ospf 11 area 1
 ip ospf network point-to-point
```

- 프로세스, Area, Router ID는 `docs/ospf-design.md` 2~3절의 표 값을 사용. 터널 인터페이스는 3.5절 참조.

### 3.3 ISP 연결(정적 기본 경로, IP SLA, 기본 경로 광고)

```
ip sla 1
 icmp-echo 100.2.12.2 source-interface gi3/0
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
crypto isakmp key dmvpn-hub address 0.0.0.0 0.0.0.0
crypto isakmp keepalive 10 3
crypto ipsec transform-set ts-aes256 esp-aes 256 esp-sha-hmac
 mode tunnel
crypto ipsec profile dmvpn-profile
 set transform-set ts-aes256
 set pfs group5
 set security-association lifetime seconds 3600
!
int tunnel0
 ip addr 172.16.0.13 255.255.255.0
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source gi4/0
 tunnel mode gre multipoint
 tunnel key 100
 ip nhrp network-id 100
 ip nhrp holdtime 600
 ip nhrp authentication nhrpkey1
 ip nhrp nhs 172.16.0.1
 ip nhrp map 172.16.0.1 203.0.113.5
 ip nhrp map multicast 203.0.113.5
 ip nhrp shortcut
 ip ospf 11 area 0
 ip ospf network broadcast
 ip ospf priority 0
 tunnel protection ipsec profile dmvpn-profile shared
```

- 예시는 Spoke `HQ-R2`. Hub(`DC-R2`)는 `ip nhrp map multicast dynamic`, `ip nhrp redirect`, `ip ospf priority 255`를 사용하고 NHS·map·shortcut 줄 제외.

### 3.6 Private Internet 언더레이와 ACL

```
router ospf 2
 router-id 11.11.11.2
int gi4/0
 ip ospf 2 area 100
 ip ospf network point-to-point
!
ip access-list extended pi-whitelist
 permit ospf any any
 permit gre 203.0.113.0 0.0.0.255 203.0.113.0 0.0.0.255
 permit esp 203.0.113.0 0.0.0.255 203.0.113.0 0.0.0.255
 permit udp 203.0.113.0 0.0.0.255 203.0.113.0 0.0.0.255 eq isakmp
 permit icmp any any
 deny ip any any log
```

- ACL은 `Private-net`의 5개 링크 인터페이스에 적용(`ip access-group pi-whitelist in`). `ICMP` 허용 줄은 임시이며 검증 후 제거.
- 사이트 라우터는 사이트 프로세스와 같은 Router ID를 프로세스 2에도 사용. `Private-net`의 Router ID는 `2.2.2.2`.

### 3.7 QoS

```
class-map match-any cm-control
 match dscp cs6
class-map match-any cm-business
 match dscp af31
!
policy-map pm-lan-out-child
 class cm-control
  bandwidth percent 10
 class cm-business
  bandwidth percent 30
 class class-default
  fair-queue
policy-map pm-lan-out
 class class-default
  shape average 10000000
  service-policy pm-lan-out-child
!
int gi0/0
 service-policy output pm-lan-out
```

- 적용 대상은 사이트 스위치와 연결된 라우터 인터페이스(`Gi0/0`, `Gi1/0`) 20포트(`docs/network-project-final.md` 5절). 제외: 라우터 간 링크, ISP 방향, PI 방향, Tunnel0.
- `shape average` 값은 병목 검증용이며 DNS(53) 트래픽은 `match access-group`으로 `cm-business`에 분류하고 `set dscp af31`로 표시. 스위치 QoS는 적용하지 않음.
