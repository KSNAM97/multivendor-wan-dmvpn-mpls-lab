# VRRP 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 2절
- 검증일: 2026-10-13
- Aruba CX 교체: BR1 스위치를 Aruba CX로 교체해 명령은 AOS-CX 기준이며, 이전 결과는 Cisco 구성 기록이라 교체 후 재확인 필요

| 장비         | 명령                | 기대 결과                                                                         | 실제 결과 | 판정  |
| ---------- | ----------------- | ----------------------------------------------------------------------------- | ----- | --- |
| BR1-SW1-L3 | `show vrrp` | Vl10 그룹 10, Vl20 그룹 20, priority 110, Master, VIP 10.3.10.1, 10.3.20.1 | (Cisco 구성 기록) Vl10 그룹 10, Vl20 그룹 20, priority 110, Master, VIP 10.3.10.1, 10.3.20.1 | 재확인 |
| BR1-SW2-L3 | `show vrrp` | Vl10 그룹 10, Vl20 그룹 20, priority 90, Backup, Master addr 10.3.10.2, 10.3.20.2 | (Cisco 구성 기록) Vl10, Vl20 priority 90, Backup, Master addr 10.3.10.2, 10.3.20.2. BR1-SW1-L3 shutdown 시 Master 전환, 복구 시 Backup 복귀 확인 | 재확인 |

BR1-SW1-L3 VRRP MASTER
![BR1-SW1-L3 VRRP MASTER](images/Pasted%20image%2020261013181724.png)

BR1-SW2-L3 VRRP BACKUP
![BR1-SW2-L3 VRRP BACKUP](images/Pasted%20image%2020261013181737.png)

BR1-SW2-L3 VRRP MASTER 전환 확인(BR1-SW1-L3 SHUTDOWN)
![BR1-SW2-L3 VRRP MASTER 전환 확인(BR1-SW1-L3 SHUTDOWN)](images/Pasted%20image%2020261013181846.png)

BR1-SW2-L3 VRRP BACKUP 전환 확인(BR1-SW1-L3 복구)
![BR1-SW2-L3 VRRP BACKUP 전환 확인(BR1-SW1-L3 복구)](images/Pasted%20image%2020261013183934.png)