# [TCP] 3 way handshake & 4 way handshake

태그: 네트워크
CS 주제: [5주차] 네트워크 (https://www.notion.so/5-8a44a8a690a248a2a237f9ff12ec3b49?pvs=21)
날짜: 2023년 8월 28일
블로그 게시: No
완료: Yes
작성자: sun
참고자료: https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake
출석: 230828 출석 (https://www.notion.so/230828-2e0b14db5675446da3cbfd861fa8344b?pvs=21)

# TCP / UDP

## TCP **(Transmission Control Protocol)**

<aside>
💡 인터넷상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜

</aside>

- TCP는 애플리케이션에게 **신뢰적이고 연결지향성** 서비스를 제공
  
    → TCP와 IP는 함께 사용되며 
  
    **IP는 배달**
  
    **TCP는 `패킷`의 추적 및 관리**를 수행

- 연결형 서비스
  
    → 신뢰적인 전송을 보장 ⇒ hanshaking하고 데이터의 흐름 제어와 혼잡 제어를 수행
  
    → 하지만 이로 인해 TCP의 속도는 느림

**특징**

- 3-way handshaking 과정을 통해 연결을 설정 / 4-way handshaking을 통해 해제
- 흐름 제어 및 혼잡 제어
- 높은 신뢰성을 보장
- UDP보다 속도가 느림
- 전이중(Full-Duplex), 점대점(Point to Point) 방식.

## UDP **(User Datagram Protocol)**

<aside>
💡 데이터를 *데이터그램 단위로 처리하는 프로토콜

(* 데이터그램: 독립적인 관계를 지니는 패킷)

</aside>

- 비연결형 프로토콜
  
    ⇒ 할당되는 논리적인 경로가 없고 각각의 패킷이 다른 경로로 전송
  
    ⇒ 이 각각의 패킷은 독립적인 관계를 지님
  
    ⇒ 이렇게 데이터를 서로 다른 경로로 독립 처리하는 프로토콜을 UDP 라고 함

- UDP는 연결을 설정하고 해제하는 과정이 없음
  
    ⇒ 서로 다른 경로로 독립적으로 처리함에도 패킷에 순서를 부여하여 재조립하거나 흐름제어 및 혼잡제어를 수행하지 않음
  
    ⇒ 따라서 속도가 빠르며 네트워크 부하가 적음
  
    ⇒ 반면, 데이터 전송의 신뢰성이 낮음 
  
    ⇒ 연속성이 중요한 실시간 서비스(스트리밍)에 좋음

**특징**

- 비연결형 서비스로 데이터그램 방식을 제공
- 정보를 주고 받을 때 정보를 보내거나 받는다는 신호 절차를 거치지 않음
- UDP 헤더의 CheckSum 필드를 통해 최소한의 오류만 검출
- 신뢰성이 낮음
- TCP보다 속도가 빠름

## 3-way and 4-way handshake

- ***포트(PORT) 상태 정보***
  
  - CLOSED: 포트가 닫힌 상태
  - LISTEN: 포트가 열린 상태로 연결 요청 대기 중
  - SYN_RCV: SYNC 요청을 받고 상대방의 응답을 기다리는 중
  - ESTABLISHED: 포트 연결 상태

- ***플래그 정보***
  
  - TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
    - 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
  - SYN(Synchronize Sequence Number) / 000010
    - 연결 설정. Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
  - ACK(Acknowledgement) / 010000
    - 응답 확인. 패킷을 받았다는 것을 의미한다.
    - Acknowledgement Number 필드가 유효한지를 나타낸다.
    - 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
  - FIN(Finish) / 000001
    - 연결 해제. 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.

## TCP vs UDP

![Untitled.png]([TCP]%203%20way%20handshake%20&%204%20way%20handshake%205cd062c2ab1d4526aa650b7897253449_assets/4fdc76c2c29a2ad03df8e29bd1719bcfaafcb417.png)



# 3 way handshake (TCP 연결 성립(접속))

<aside>
💡 TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 **연결을 설정(Connection Establish)** 하는 과정

- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장
  
    ⇒ 실제로 데이터 전달 시작 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 함

- 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에, 정확한 전송 보장을 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미
  
  </aside>

### 매커니즘

![Untitled 1.png]([TCP]%203%20way%20handshake%20&%204%20way%20handshake%205cd062c2ab1d4526aa650b7897253449_assets/3b190a1c13d569bcf3818f0b443ec1b6f6271d95.png)

⇒ TCP 통신은 PAR (Positive Acknowledgement with Re-transmission) 을 통해 신뢰적인 통신을 제공

**PAR을 사용하는 기기는 ack을 받을 때까지 데이터 유닛을 재전송한다**

→ 수신자가 데이터 유닛(세그먼트) 손상 확인

→ 해당 세그먼트 삭제

→ sender는 positive ack이 오지 않은 데이터 유닛을 재전송

⇒ 이 과정에서 클라이언트와 서버 사이에서 3개의 Segment가 교환

### 작동 방식

- Client는 Server와 연결을 위해 3-way handshake를 통해 연결 요청

- 일반적인 Client와 Server는 모두 서로 연결 요청을 먼저 할 수 있음
  
    → 연결 요청을 먼저 시도한 요청자 : Clien
  
    → 연결 요청을 받은 수신자 : Server

- **SYN(Synchronization)**
  
    : 연결요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 전송

- **ACK(Acknowledgement)**
  
    : 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송

# 4 way handshake (TCP 연결 해제)

<aside>
💡 연결을 해제 (Connecntion Termination)하는 과정

- 여기서는 FIN 플래그를 이용
  
  - `FIN` (finish)
    
      → 세션을 종료 시키는 데 사용, 더 이상 보낸 데이터가 없음을 나타

</aside>

### Termination 종류

1. **Graceful connection release(정상적인 연결 해제**)
   
    정상적인 연결 해제에서는 양쪽이 커넥션이 서로 모두 커넥션을 닫을 때까지 연결됨

2. **Abrupt connection release(갑작스런 연결 해제**)
   
   - 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우
   - 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우

### 작동 방식 (Abrupt)

`RST(TCP reset)` 세그먼트가 전송되면 갑작스러운 연결 해제가 수행

→ RST 세그먼트는 다음과 같은 경우에 전송

1. 존재하지 않는 TCP 연결에 대해 비SYN 세그먼트가 수신된 경우

2. 열린 커넥션에서 일부 TCP 구현은 잘못된 헤더가 있는 세그먼트가 수신된 경우
   
   - RST 세그먼트를 보내, 해당 커넥션을 닫아 공격을 방지

3. 일부 구현에서 기존 TCP 연결을 종료해야 하는 경우
   
   - 연결을 지원하는 리소스가 부족할 때
   - 원격 호스트에 연결할 수 없고 응답이 중지되었을 때

### 작동 방식 (Graceful)

일반적인 Client와 Server는 모두 서로 연결 요청을 먼저 할 수 있음

→ 연결 요청을 먼저 시도한 요청자 : Client

→ 연결 요청을 받은 수신자 : Server

<aside>
💡 **Half-Close 기법**

- 연결을 종료하려고 할 때 완전히 종료하지 않고 `반만 종료`

- Half-Close 기법을 사용하면 종료 요청자가 처음 보내는 FIN 패킷에 승인 번호를 함께 담아서 보냄
  
    ⇒ 승인 번호의 의미
    : **"일단 연결은 종료하지만, 귀는 열어둘게. 이 승인 번호까지 처리했으니까 더 보낼 거 있으면 보내"**

- 이후 수신자가 남은 데이터를 모두 보내고 나면 다시 요청자에게 **FIN 패킷**을 보냄으로써 모든 데이터가 처리되었다는 신호 **FIN**를 보냄
  
    ⇒ 요청자는 그때 나머지 반을 닫으면서 좀 더 안전하게 연결을 종료

</aside>