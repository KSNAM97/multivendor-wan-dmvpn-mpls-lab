# VRRP 검증 (BR1 / distribution)

- Jira: KAN-12
- 대상 장비: BR1-SW1-L3, BR1-SW2-L3
- 기준 문서: [vrrp-mstp-design.md](../../../docs/vrrp-mstp-design.md) 2절
- 검증일: 2026-10-13

| 장비         | 명령                | 기대 결과                                                                         | 실제 결과 | 판정  |
| ---------- | ----------------- | ----------------------------------------------------------------------------- | ----- | --- |
| BR1-SW1-L3 | `show vrrp brief` | Vl10 그룹 10, Vl20 그룹 20, priority 110, Master, VIP 10.3.10.1, 10.3.20.1 | Vl10 그룹 10, Vl20 그룹 20, priority 110, Master, VIP 10.3.10.1, 10.3.20.1 | 통과 |
| BR1-SW2-L3 | `show vrrp brief` | Vl10 그룹 10, Vl20 그룹 20, priority 90, Backup, Master addr 10.3.10.2, 10.3.20.2 | Vl10, Vl20 priority 90, Backup, Master addr 10.3.10.2, 10.3.20.2. BR1-SW1-L3 shutdown 시 Master 전환, 복구 시 Backup 복귀 확인 | 통과 |
BR1-SW1-L3 VRRP MASTER
![BR1-SW1-L3 VRRP MASTER](images/Pasted%20image%2020261013181724.png)

BR1-SW2-L3 VRRP BACKUP
![BR1-SW2-L3 VRRP BACKUP](images/Pasted%20image%2020261013181737.png)

BR1-SW2-L3 VRRP MASTER 전환 확인(BR1-SW1-L3 SHUTDOWN)
![BR1-SW2-L3 VRRP MASTER 전환 확인(BR1-SW1-L3 SHUTDOWN)](images/Pasted%20image%2020261013181846.png)

BR1-SW2-L3 VRRP BACKUP 전환 확인(BR1-SW1-L3 복구)
![BR1-SW2-L3 VRRP BACKUP 전환 확인(BR1-SW1-L3 복구)](images/Pasted%20image%2020261013183934.png)