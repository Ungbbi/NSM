## 👩‍💻 팀원 소개



| 노솔리 | 박웅빈 | 이승준 | 이승언 |
|:-----------:|:-----------:|:-----------:|:-----------:|
| <img width="100px" src="https://avatars.githubusercontent.com/soljjang777" /> | <img width="100px" src="https://avatars.githubusercontent.com/Ungbbi" /> | <img width="100px" src="https://avatars.githubusercontent.com/leesj000603"/> | <img width="100px" src="https://avatars.githubusercontent.com/seungunleeee"/> |
| [@soljjang777](https://github.com/soljjang777) | [@Ungbbi](https://github.com/Ungbbi) | [@leesj000603](https://github.com/leesj000603) | [@seungunleeee](https://github.com/seungunleeee) |

<br><br>

# 고가용성을 위한 네트워크 구성 요소 이중화 
PC-A에서 PC-B로 ICMP 패킷을 보낼 때 라우터가 갑자기 고장나거나 라우터의 인터페이스가 물리적, 논리적 문제로 인해 다운된다면❓❓<br> 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/efef5140-b2e1-49c4-b5e7-915f0f1c1de0" width="400">
</div>

PC-A에는 PC-B에 ICMP 패킷이 보내 질 때까지 한참을 기다리거나 최악의 경우 편지를 못 보낼 수도 있음❗
<br><br>

만약 이 상황이 하나의 기업이라면❓❓<br> 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/c969e5e2-f905-4b51-9c99-96de6253290d" width="400">
</div>

업무가 지연되거나 최악의 경우 완전히 중단되면 기업의 경우는 심각한 운영상의 손실로 이어짐❗

**즉, 네트워크의 안정성과 가용성을 보장하여 서비스 중단을 방지하고, 비즈니스 연속성을 유지하기 위해서 이중화는 필수이다. <br>
네트워크의 장애 발생 시에도 백업 경로나 장치를 통해 트래픽을 지속적으로 전달할 수 있도록 구성하여 네트워크의 안정성과 가용성을 보장해야한다.**
<br><br>

# 네트워크 장비 이중화
- 고가용성을 보장하는 네트워크 인프라 구축 
- 주 라우터(Active Router) 장애 시, 대기 라우터(Standby Router)로의 자동 전환
- 네트워크 다운타임 최소화 및 서비스 연속성 보장
<br><br>

# HSRP란? (Hot Standby Routing Protocol)
HSRP는 시스코에서 개발한 프로토콜로, 두 대 이상의 라우터나 멀티레이어 스위치가 하나의 가상 IP 주소와 맥 주소를 공유하여<br> 네트워크의 기본 게이트웨이 역할을 하도록 함.<br>
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/2ec38be4-17df-47d8-9f0a-1e83cf724042" width="450">
</div><br>
네트워크 상의 모든 클라이언트는 이 가상 IP 주소를 통해 트래픽을 라우팅하므로,
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/abd21d64-3397-46df-bab1-c88cb5c10130" width="450">
</div><br>
HSRP로 설정된 장비 중 하나가 장애가 발생하더라도 다른 장비가 자동으로 이 역할을 이어받아 트래픽을 처리함.
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/6c4c8f02-6de5-4e65-9c31-4d40168f57db" width="450">
</div><br>

**이를 통해 네트워크의 가용성을 크게 향상시킬 수 있다.**

<br><br>

# 시스템 구성도 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/eac3495c-b6ac-473c-9609-9574422e3727" width="850">
</div>
<br><br>

# HSRP 설정 및 구성 
### 🔵PC1 IP 설정
```cisco
ip 192.168.1.2/24 192.168.1.1
```
### 🟢PC1 IP 설정 확인
```cisco
PC1> sh ip

NAME        : PC1[1]
IP/MASK     : 192.168.1.2/24
GATEWAY     : 192.168.1.1

...
```
### 🔵ISP Interface IP 설정 및 Static Routing
```cisco
int e0/0
 ip address 10.1.1.2 255.255.255.0
 no sh
ip route 192.168.1.0 255.255.255.0 e0/0
```
### 🟢ISP Interface IP 설정 확인
```cisco
ISP#sh ip ro
...
      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.1.1.0/24 is directly connected, Ethernet0/0
L        10.1.1.2/32 is directly connected, Ethernet0/0
S     192.168.1.0/24 is directly connected, Ethernet0/0


ISP#sh ip int b
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.1.1.2        YES manual up                    up
...
```
### 🔵R1 Interface IP 설정
```cisco
interface e0/1
  ip address 192.168.1.254 255.255.255.0
  no sh
interface e0/0
  ip address 10.1.1.254 255.255.255.0
  no sh
```
### 🟢R1 Interface IP 설정 확인
```cisco
R1#sh ip int b
Interface                  IP-Address      OK? Method Status                Prot        ocol
Ethernet0/0                10.1.1.254      YES manual up                    up          
Ethernet0/1                192.168.1.254   YES manual up                    up 
```
### 🔵R2 Interface IP 설정
```cisco
interface e0/2
  ip address 192.168.1.253 255.255.255.0
  no sh
interface e0/0
  ip address 10.1.1.253 255.255.255.0
  no sh
```
### 🟢R2 Interface IP 설정 확인
```cisco
R2#sh ip int b
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.1.1.253      YES manual up                    up  
Ethernet0/1                unassigned      YES NVRAM  administratively down down
Ethernet0/2                192.168.1.253   YES manual up                    up
...
```
### 🔵R1 HSRP 설정
```cisco
interface e0/1
  standby 1 ip 192.168.1.1
  standby 1 priority 100
  standby 1 preempt
```
### 🟢R1 HSRP 설정 확인
```cisco
R1#sh standby
Ethernet0/1 - Group 1
  State is Speak
  Virtual IP address is 192.168.1.1
  Active virtual MAC address is unknown
    Local virtual MAC address is 0000.0c07.ac01 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 2.768 secs
  Preemption enabled
  Active router is unknown
  Standby router is unknown
  Priority 100 (default 100)
  Group name is "hsrp-Et0/1-1" (default)

```
### 🔵R2 HSRP 설정
```cisco
interface e0/2
  standby 1 ip 192.168.1.1
  standby 1 priority 95
  standby 1 preempt
```
### 🟢R2 HSRP 설정 확인
```cisco
R2#sh standby
Ethernet0/2 - Group 1
  State is Listen
  Virtual IP address is 192.168.1.1
  Active virtual MAC address is 0000.0c07.ac01
    Local virtual MAC address is 0000.0c07.ac01 (v1 default)
  Hello time 3 sec, hold time 10 sec
  Preemption enabled
  Active router is 192.168.1.254, priority 100 (expires in 8.976 sec)
  Standby router is unknown
  Priority 95 (configured 95)
  Group name is "hsrp-Et0/2-1" (default)

```
### 🔵R1 Track 설정 및 적용
```cisco
ip sla 1
  icmp-echo 10.1.1.2
  frequency 5
  exit
ip sla schedule 1 life forever start-time now
track 1 ip sla 1 reachability

int e0/1
 standby 1 track 1
```
### 🟢R1 Track 설정 확인
```cisco
R1#sh track
Track 1
  IP SLA 1 reachability
  Reachability is Up
    2 changes, last change 00:00:59
  Latest operation return code: OK
  Latest RTT (millisecs) 1
  Tracked by:
    HSRP Ethernet0/1 1

R1#sh standby
...
    Track object 1 state Up decrement 10
```
### 🔵R2 Track 설정 및 적용
```cisco
ip sla 1
  icmp-echo 10.1.1.2
  frequency 5
  exit
ip sla schedule 1 life forever start-time now
track 1 ip sla 1 reachability

int e0/2
 standby 1 track 1
```
### 🟢R2 Track 설정 확인
```cisco
R2#sh track
Track 1
  IP SLA 1 reachability
  Reachability is Up
    2 changes, last change 00:00:38
  Latest operation return code: OK
  Latest RTT (millisecs) 1
  Tracked by:
    HSRP Ethernet0/2 1

R2#sh standby
...
    Track object 1 state Up decrement 10

```
<br><br>

# 테스트
1. PC1 -> ISP로 ICMP 전송 및 경로 확인
```cisco
PC1> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=254 time=0.909 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=254 time=0.448 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=254 time=0.511 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=254 time=0.681 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=254 time=0.435 ms

PC1> trace  10.1.1.2
trace to 10.1.1.2, 8 hops max, press Ctrl+C to stop
 1   192.168.1.254   0.328 ms  0.141 ms  0.139 ms
 2   *10.1.1.2   0.926 ms (ICMP type:3, code:3, Destination port unreachable)
```
2. 

<br><br>

# 💥트러블 슈팅
### 상황 1. preempt 설정 부재 
라우터에서 HSRP 구성 시 preempt 설정이 없는 경우, 다음과 같은 문제가 발생:
   ```cisco
   Router(config-if)# standby <group-number> preempt
   ```

* 라우터의 인터페이스에 장애가 발생했을 때 Active 라우터의 우선순위(priority) 값이 감소하고, Standby 라우터가 Active 라우터보다 높은 우선순위를 가지게 되면서 Active로 변경 됨.
* 그러나 preempt 기능이 설정되어 있지 않으면, Standby 라우터가 Active 역할을 자동으로 가져가지 않음. 이로 인해, 네트워크에서 장애가 복구되지 않음.<br>
  
**따라서, HSRP 구성 시 preempt 설정을 통해 우선순위가 높은 라우터가 자동으로 Active 역할을 할 수 있도록 설정하는 것이 중요함.**
<br><br>

### 상황 2. Line Protocol 트래킹의 한계
아래와 같이 Line Protocol을 사용하여 R1과 R2에 연결된 인터페이스의 트래킹을 설정했으나, 이는 다음과 같은 한계를 가짐 :
   ```cisco
   R1(config)#track 1 interface e0/0 line-protocol
   ```
   ```cisco
   R2(config)#track 1 interface e0/0 line-protocol
   ```
* Line Protocol 트래킹은 라우터의 인터페이스 상태(업/다운)만을 감시할 수 있음.
* 실제로 통신을 원하는 목적지 장비와의 연결 상태(Reachability)를 추적하지는 못함. <br>

**이러한 한계를 해결하기 위해 IP SLA를 사용하여, 특정 경로의 연결 상태를 ICMP 패킷을 통해 주기적으로 모니터링하는 방법을 결정함.** <br>
```cisco
R1(config-if)#ip sla 1
R1(config-ip-sla)#icmp-echo 10.1.1.2
R1(config-ip-sla-echo)#frequency 5
R1(config-ip-sla-echo)#exit
R1(config)#ip sla schedule 1 life forever start-time now
R1(config)#track 1 ip sla 1 reachability

R1(config-track)#int e0/1
R1(config-if)# standby 1 track 1
```
```cisco
R2(config-if)#ip sla 1
R2(config-ip-sla)#icmp-echo 10.1.1.2
R2(config-ip-sla-echo)#frequency 5
R2(config-ip-sla-echo)#exit
R2(config)#ip sla schedule 1 life forever start-time now
R2(config)#track 1 ip sla 1 reachability

R2(config-track)#int e0/2
R2(config-if)# standby 1 track 1
```

* IP SLA 프로세스를 생성하여 원하는 경로에 대해 ICMP 에코 요청을 주고받도록 설정함.
* 생성된 IP SLA 프로세스를 트랙(Track) 객체에 등록하여, 실시간으로 경로의 연결 상태를 보다 정밀하게 감시할 수 있도록 구성함. <br>

**이 설정을 통해, 네트워크 경로의 상태를 정확히 추적하고 네트워크의 가용성을 보장할 수 있음.**
<br><br>

# 사용된 네트워크 모니터링 도구 및 소프트웨어 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/518690f3-c6fc-49c7-aa31-be5739cf61e6" width="450">
</div>

 **네트워크 모니터링 도구 : WireShark Version 4.0.3** <br>
 **사용된 소프트웨어 : Gns3 Version 2.2.39**
<br><br>

# 추후 계획  
HSRP는 기본적으로 한 라우터만 활성 상태로 처리하기 때문에 로드밸런싱(sharing)을 구현하기 위해서 추후 Multiple Group HSRP를 사용해 보고자 한다. <br>
![image](https://github.com/user-attachments/assets/7434a44d-4296-4dda-a2a4-b8db67b812c9)


# 회고
| 이름   | 코멘트   |
|--------|----------|
| 노솔리 | HSRP에 대해 공부하면서 네트워크 이중화의 중요성을 한 번 인식 하고 갈 수 있어서 좋았고 구현을 통해 다양한 지식을 습득할 수 있어서 좋았습니다. |
| 박웅빈 | HSRP 프로토콜에 대해 공부하며 패킷의 전송 과정과 라우터, 스위치 간 작동 방식을  알게되며 어떻게 이중화가 구현되는지 알게되었습니다. HSRP 프로토콜 뿐만 아니라 네트워크 개념을 복습 및 보완 했어서 좋았습니다. |
| 이승준 | HSRP를 사용한 라우터 이중화를 구현하면서 상태 변화, 선출 과정, 트래킹, ip sla, hello packet, GARP에 대한 지식을 습득 할 수 있었고 상태 전환과정을 이해할 수 있었습니다. |
| 이승언 | 이중화를 해야할 경우 고려해야할 다양한 점들을 경험할 수 있었고 HSRP가 어떤 프로토콜들을 사용해 동작하는 지 확인할 수 있었습니다. |

