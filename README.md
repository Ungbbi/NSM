## 👩‍💻 팀원 소개



|                                         노솔리                                         |                                      박웅빈                                      |                                        이승준                                        |                                         이승언                                          |
| :-------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------: |
| <img  width="100px" src="https://avatars.githubusercontent.com/soljjang777" /> | <img width="100px" src="https://avatars.githubusercontent.com/Ungbbi" /> | <img width="100px" src="https://avatars.githubusercontent.com/leesj000603"/> |     <img width="100px" src="https://avatars.githubusercontent.com/seungunleeee"/>     |
|                       [@soljjang777](https://github.com/soljjang777)                        |           [@Ungbbi](https://github.com/Ungbbi)           |                      [@leesj](https://github.com/leesj000603)                      |                    [@seungunleeee](https://github.com/seungunleeee)                     |

# 고가용성을 위한 네트워크 구성 요소 이중화 
▶PC-A에서 PC-B로 편지를 보낼 때 라우터가 갑자기 고장나거나 라우터의 인터페이스가 물리적, 논리적 문제로 인해 다운된다면❓❓<br> 
▶PC-A에는 PC-B에 편지가 보내 질 때까지 한참을 기다리거나 최악의 경우 편지를 못 보낼 수도 있음❗<br> 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/efef5140-b2e1-49c4-b5e7-915f0f1c1de0" width="400">
</div>
<br><br>
▶만약 이 상황이 하나의 기업이라면❓❓<br> 
▶업무가 지연되거나 최악의 경우 완전히 중단되면 기업의 경우는 심각한 운영상의 손실로 이어짐❗<br> 
<div style="display: inline-block;">
  <img src="https://github.com/user-attachments/assets/c969e5e2-f905-4b51-9c99-96de6253290d" width="400">
</div>
<br><br>
즉, 네트워크의 안정성과 가용성을 보장하여 서비스 중단을 방지하고, 비즈니스 연속성을 유지하기 위해서 이중화는 필수이다. <br>
네트워크의 장애 발생 시에도 백업 경로나 장치를 통해 트래픽을 지속적으로 전달할 수 있도록 구성하여 네트워크의 안정성과 가용성을 보장해야한다.
# 2. 프로젝트 목표 (Objectives)
고가용성을 보장하는 네트워크 인프라 구축
주 라우터(Active Router) 장애 시, 대기 라우터(Standby Router)로의 자동 전환
네트워크 다운타임 최소화 및 서비스 연속성 보장
# 3. 시스템 구성도 (Architecture Diagram)
HSRP 구성된 네트워크의 다이어그램 (Active/Standby 라우터, 스위치, 클라이언트 위치 등)
기본적으로 사용되는 장비들의 명세 (라우터, 스위치 등)
주요 네트워크 구성요소 (VLAN, IP 서브넷 등)
# 4. HSRP 설정 및 구성 단계 (HSRP Configuration Steps)
라우터에서 HSRP 설정 방법 (실제 설정 예시 포함)
bash
interface GigabitEthernet0/1
  ip address 192.168.1.1 255.255.255.0
  standby 1 ip 192.168.1.254
  standby 1 priority 110
  standby 1 preempt
Active Router와 Standby Router의 역할 설명
HSRP 우선순위와 프리엠프 설정
HSRP 가상 IP 구성 방법
# 5. 네트워크 구성 테스트 (Testing the Network Configuration)
HSRP 전환 시나리오 테스트
Active 라우터가 비정상 동작할 때 Standby 라우터로의 자동 전환 여부
장애 발생 시 서비스 연속성 테스트
네트워크 성능 테스트 (전환 시간, 장애 복구 시간 등)
# 6. 사용된 네트워크 장비 및 소프트웨어 (Hardware and Software Used)
사용된 라우터 및 스위치 모델과 버전
네트워크 관리 및 모니터링 도구
기타 사용된 소프트웨어(예: 시뮬레이션 도구, 네트워크 모니터링 소프트웨어)
# 7. 고려사항 및 최적화 (Considerations and Optimization)
HSRP 구성 시 주요 고려사항 (예: HSRP 그룹, 우선순위 설정, preempt 기능 등)
네트워크 트래픽 관리 및 부하 분산
장애 상황에서의 전환 시간 최적화 방안
보안적인 측면에서의 설정(HSRP 인증 등)
# 8. 장애 발생 시나리오 및 해결책 (Failure Scenarios and Solutions)
주요 장애 시나리오 (예: Active 라우터 다운, 네트워크 장애 등)
장애 복구 과정 설명
HSRP 전환 후 네트워크 복구 절차
# 9. 성과 및 결과 (Results and Outcomes)
네트워크 가용성 향상 결과
프로젝트 완료 후 측정된 네트워크 안정성 및 성능 향상
장애 복구 시간과 가동률
# 10. 향후 계획 및 개선 방안 (Future Plans and Improvements)
향후 네트워크 이중화에 대한 추가 기능 도입 가능성 (예: VRRP, GLBP 등)
네트워크 보안 강화 방안
기타 최적화 전략
# 11. 참고 자료 (References)
HSRP 관련 공식 문서 및 참고 자료
네트워크 이중화 관련 학술 자료 및 논문
관련 RFC 문서 (RFC 2281)
# 12. 회고

