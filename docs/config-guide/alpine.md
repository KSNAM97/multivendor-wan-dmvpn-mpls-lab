# 알파인 노드 설정

> 설정 가이드 목차: [README.md](./README.md) | [Cisco 라우터](./cisco-router.md) | [Cisco 스위치](./cisco-switch.md) | [Arista 스위치](./arista-switch.md) | [알파인](./alpine.md)

- `DC-DNS-1`, `DC-DNS-2`: `docs/dhcp-dns-design.md` 2절의 `interfaces`, `dnsmasq.conf`, `hosts.lab` 사용.
- 호스트(`HQ-PC*`, `BR*-PC*`): DHCP 클라이언트(`docs/dhcp-dns-design.md` 4절). 별도 컨피그 파일 없음.

- 호스트 `/etc/network/interfaces`(DHCP 클라이언트):

```
auto eth0
iface eth0 inet dhcp
```

- 서버(`DC-DNS-1`, `DC-DNS-2`)의 고정 주소 설정과 `dnsmasq.conf`는 `docs/dhcp-dns-design.md` 2.1절 형식 사용.
