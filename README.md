## 👩‍💻 팀원 소개



|                                         노솔리                                         |                                      박웅빈                                      |                                        이승준                                        |                                         이승언                                          |
| :-------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------: |
| <img  width="100px" src="https://avatars.githubusercontent.com/soljjang777" /> | <img width="100px" src="https://avatars.githubusercontent.com/Ungbbi" /> | <img width="100px" src="https://avatars.githubusercontent.com/leesj000603"/> |     <img width="100px" src="https://avatars.githubusercontent.com/seungunleeee"/>     |
|                       [@soljjang777](https://github.com/soljjang777)                        |           [@Ungbbi](https://github.com/Ungbbi)           |                      [@leesj](https://github.com/leesj000603)                      |                    [@seungunleeee](https://github.com/seungunleeee)                     |

# 고가용성을 위한 네트워크 구성 요소 이중화 
▶PC-A에서 PC-B로 ICMP 패킷을 보낼 때 라우터가 갑자기 고장나거나 라우터의 인터페이스가 물리적, 논리적 문제로 인해 다운된다면❓❓<br> 
▶PC-A에는 PC-B에 ICMP 패킷이 보내 질 때까지 한참을 기다리거나 최악의 경우 편지를 못 보낼 수도 있음❗<br> 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/efef5140-b2e1-49c4-b5e7-915f0f1c1de0" width="400">
</div>
<br>
▶만약 이 상황이 하나의 기업이라면❓❓<br> 
▶업무가 지연되거나 최악의 경우 완전히 중단되면 기업의 경우는 심각한 운영상의 손실로 이어짐❗<br> 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/c969e5e2-f905-4b51-9c99-96de6253290d" width="400">
</div>
<br>
즉, 네트워크의 안정성과 가용성을 보장하여 서비스 중단을 방지하고, 비즈니스 연속성을 유지하기 위해서 이중화는 필수이다. <br>
네트워크의 장애 발생 시에도 백업 경로나 장치를 통해 트래픽을 지속적으로 전달할 수 있도록 구성하여 네트워크의 안정성과 가용성을 보장해야한다.
<br><br>

# 프로젝트 목표 (Objectives)
- 고가용성을 보장하는 네트워크 인프라 구축 (네트워크 장비 이중화)
- 주 라우터(Active Router) 장애 시, 대기 라우터(Standby Router)로의 자동 전환
- 네트워크 다운타임 최소화 및 서비스 연속성 보장
<br><br>

# 시스템 구성도 (Architecture Diagram)
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/73bcca73-c0f4-4415-a29f-5080370cac86" width="650">
</div>
<br><br>

# HSRP 설정 및 구성 단계 (HSRP Configuration Steps)
### PC1 IP 설정
```bash
ip 192.168.1.1/24 192.168.1.252
```
### ISP IP 설정
```bash
ip 10.1.1.1/24 10.1.1.252
```
### R1 인터페이스 IP 설정
```bash
interface e0/1
  ip address 192.168.1.254 255.255.255.0
  no sh
interface e0/2
  ip address 10.1.1.254 255.255.255.0
  no sh
```
### R2 인터페이스 IP 설정
```bash
interface e0/2
  ip address 192.168.1.253 255.255.255.0
  no sh
interface e0/1
  ip address 10.1.1.253 255.255.255.0
  no sh
```
### R1 HSRP 설정
```bash
interface e0/1
  standby 1 ip 192.168.1.252
  standby 1 priority 100
  standby 1 preempt
interface GigabitEthernet0/2
  standby 2 ip 10.1.1.252
  standby 2 priority 100
  standby 2 preempt
```
### R2 HSRP 설정
```bash
interface e0/2
  standby 1 ip 192.168.1.252
  standby 1 priority 100
  standby 1 preempt
interface GigabitEthernet0/1
  standby 2 ip 10.1.1.252
  standby 2 priority 100
  standby 2 preempt
```
<br><br>

# 네트워크 구성 테스트 (Testing the Network Configuration)
 * HSRP 전환 시나리오 테스트<br>
 * Active 라우터가 비정상 동작할 때 Standby 라우터로의 자동 전환 여부<br>
 * 장애 발생 시 서비스 연속성 테스트<br>
 * 네트워크 성능 테스트 (전환 시간, 장애 복구 시간 등)<br>
<br><br>

# 사용된 네트워크 장비 및 소프트웨어 (Hardware and Software Used)
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/518690f3-c6fc-49c7-aa31-be5739cf61e6" width="450">
</div>
 * 네트워크 모니터링 도구 : WireShark Version 4.0.3 <br>
 * 사용된 소프트웨어 : Gns3 Version 2.2.39
<br><br>

# 11. 참고 자료 (References)
HSRP 관련 공식 문서 및 참고 자료
네트워크 이중화 관련 학술 자료 및 논문
관련 RFC 문서 (RFC 2281)

# 12. 회고

