# BR1

GNS3 토폴로지(검증 대상 장비와 포트). 상세 결과는 아래 문서 링크에서 확인.

![BR1 토폴로지](../../.gitbook/assets/topology-br1.png)

## 핵심 결과

* VRRP 장애 전환(Aruba): 약 4.0초 → [vrrp-failover](../../undefined-4/vrrp-failover.md)
* LAG 1 링크 다운: 손실 5개, 최대 응답 5015 ms → [etherchannel-link-down](../../undefined-4/etherchannel-link-down.md)
* L2 루프와 VRRP 광고 폭주(STP 재시작으로 해결) → [troubleshooting-log](../../undefined-5/troubleshooting-log.md)

## 계층별 검증 문서

| 계층           | 항목              | Jira   | 문서                                            |
| ------------ | --------------- | ------ | --------------------------------------------- |
| access(L2)   | STP             | KAN-12 | [access/stp.md](stp.md)                       |
| access(L2)   | VLAN, 트렁크 검증    | KAN-12 | [access/vlan-trunk.md](vlan-trunk.md)         |
| core         | DMVPN Spoke 검증  | KAN-22 | [core/dmvpn.md](dmvpn.md)                     |
| core         | ISP 연결 검증       | KAN-18 | [core/isp-link.md](isp-link.md)               |
| core         | OSPF 검증         | KAN-14 | [core/ospf.md](ospf.md)                       |
| core         | QoS 검증          | KAN-25 | [core/qos.md](qos.md)                         |
| distribution | DHCP 검증         | KAN-14 | [distribution/dhcp.md](dhcp.md)               |
| distribution | OSPF 검증         | KAN-14 | [distribution/ospf.md](ospf-1.md)             |
| distribution | EtherChannel 검증 | KAN-12 | [distribution/po-channel.md](po-channel.md)   |
| distribution | STP             | KAN-12 | [distribution/stp.md](stp-1.md)               |
| distribution | VLAN, 트렁크 검증    | KAN-12 | [distribution/vlan-trunk.md](vlan-trunk-1.md) |
| distribution | VRRP 검증         | KAN-12 | [distribution/vrrp.md](vrrp.md)               |

상세 서식과 상태는 [검증 README](../verification.md) 참조.
