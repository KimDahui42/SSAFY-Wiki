# TCP/IP flow-control & congestion-control

태그: 네트워크
CS 주제: [6주차]네트워크 (https://www.notion.so/6-53e001231b7c404f9dbf2743e38a89ec?pvs=21)
날짜: 2023년 9월 3일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: http://www.ktword.co.kr/test/view/view.php?m_temp1=5536, https://gmlwjd9405.github.io/2018/07/06/design-pattern.html, https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html, https://www.brianstorti.com/tcp-flow-control/
출석: 230904 출석 (https://www.notion.so/230904-dc4e0d97355f4b5792fa9ecf102a014c?pvs=21)

> TCP란? transmission control protocol
> 

unreliable하다는 특성을 가진 네트워크 위에서, **reliable**한 소통을 할 채널을 만들어주는 **프로토콜**

▷ network가 unreliable 하다는 건 무슨 뜻인가?

- 데이터를 노드 간에 전송할 때, 잃어버리거나 순서가 뒤섞여 오는 등의 오류가 발생한다는 것.

> 흐름제어 flowcontrol 란?
> 
- TCP가 제공하는 주요 서비스 중 하나.
- sender 가 데이터를 전송할 때 receiver가 **감당 가능한 속도**로 보내게 하겠다. → 받는 노드가 주는 노드에게 현재 상황에 대한 **피드백을** 보냄으로써 가능
- **혼잡제어** congestion control 과 혼동하지 말것

> 어떻게 작동할까?
> 

![https://blog.kakaocdn.net/dn/BE6uz/btssSCEQZoT/VnuDc7KFwGgZBPcWeNrly1/img.png](https://blog.kakaocdn.net/dn/BE6uz/btssSCEQZoT/VnuDc7KFwGgZBPcWeNrly1/img.png)

https://www.brianstorti.com/tcp-flow-control/

네트워크로 데이터를 보낼 때의 과정

- sender application은 소켓에 데이터를 쓴다.
- transport layer (TCP) 는 세그먼트에 데이터를 wrapping 하여
- 네트워크 계층으로 보낸다. (IP)
- 네트워크 계층에서는 이 패킷을 receiving node로 라우팅 한다.
- 네트워크 계층은 이 데이터를 TCP로 전달한다
- 이로써 receiver application 이 그대로의 복사본을 받을 수 있게 함
    1. TCP 는 **send buffer** 로 보내야 하는 데이터와 **receive buffer** 로 받아야 하는 데이터를 저장함.
    2. 그러면 application이 준비됐을때 application은 receive buffer로부터 데이터를 읽을것임.

☆ 요약하자면 흐름제어는, receive buffer가 이미 꽉 차있는 상태에서 패킷을 보내지 않겠다는 것이다.

---

> rwnd Receive Window
> 

TCP 가 보낼 수 있는 데이터의 양을 컨트롤하기 위해서 receiver는 **Receive window (rwnd)**를 사용함.

== TCP receive buffer의 여분 공간.

TCP는 패킷을 전달 받을 때마다, sender 에게 ack 메시지를 보내야 한다.

→ 올바르게 패킷을 받았다는 표시 + 현재 리시브 윈도우의 값을 같이 보냄

> 슬라이딩 윈도우
> 

TCP 는 슬라이딩 윈도우 프로토콜을 사용함

![https://blog.kakaocdn.net/dn/c5uqyL/btssSotcv4V/DkA0yBm3lSz2XrB5QRlhkK/img.png](https://blog.kakaocdn.net/dn/c5uqyL/btssSotcv4V/DkA0yBm3lSz2XrB5QRlhkK/img.png)

10개의 패킷을 보낸다고 가정 - 3번이 ack 되면 윈도우를 슬라이드해서 8을 보내기 시작

sender 는 항상 이 규칙을 지켜야 함.

> LastByteSend - LastByteAcked <= ReceiveWindowAdvertised
> 

---

> 혼잡제어 congestion control 란?
> 

네트워크의 혼잡 상태를 파악하고 그 상태를 해결하기 위해 데이터 전송을 제어하는 것.

→ 송신 측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법

> 혼잡이란?
> 

네트워크 내에 패킷의 수가 과도하게 증가하는 현상

> 혼잡 징후의 판단 요소
> 
- TCP 타이머 (타임아웃)
- 중복 ack
- 왕복 시간 세그먼트 지연

> 혼잡 제어 방식
> 

> AIMD(Addictive Increase Multicative Decrease)
> 

![https://blog.kakaocdn.net/dn/dLCpNE/btss74NcrsE/HUtvzf5Aov6HxcxCcf0qLk/img.png](https://blog.kakaocdn.net/dn/dLCpNE/btss74NcrsE/HUtvzf5Aov6HxcxCcf0qLk/img.png)

  https://code-lab1.tistory.com/30

- sender 가 transmission rate(window size)를 패킷 손실이 일어날 때까지 증가시키는 식의 접근법

▷ addictive increase : sender 측의 window size를 손실을 감지할때까지 매 RTT 마다 1 MSS 씩 증가시킨다.

▷ multiplicative decrease : 손실을 감지했다면 sender 측의 window size 를 절반으로 감소시킨다

1. **제한된** 형태의 혼잡제어

> RTT (왕복 시간) 편차를 고려함
> 
- 최근에 계산된 RTT 값에 균형을 주도록 작은 RTTVAR(RTT편차)의 4배 취한 값을 더함
- RTT - round trip time : 송신지부터 목적지 호스트까지 패킷이 왕복하는 데에 걸리는 시간

> 카른 알고리즘 Karn algorithm (뭔지 잘 모르겠다)
> 
- 재전송 세그먼트는 RTT 측정 대상에서 제외

> 지수 RTO 후퇴
> 
- 재전송이 일어난 경우에, 혼잡제어를 하기 위해, RTO 를 두배씩 늘려가는 방식

2. **포괄적** 형태의 혼잡제어

> 느린 시작 slow start  : 지수 증가
> 
- 혼잡 발생 상황을 미리 예방하기 위해, 세그먼트 송신율을 사전 억제
- 연결 초기에 **작게 시작** → 점차 **지수적** 상승으로 송신 데이터량을 늘림
- ▷ 패킷을 하나만 보내면서 시작. 패킷이 문제없이 도착하면 ack 패킷마다 window size를 1씩 늘려준다.

> 혼잡 회피 congestion avoidance : 가산 증가
> 
- 혼잡이 감지되면, **지수적**이 아닌 **가산적**인 증가 방식을 채택

> 빠른 재전송 fast retransmit
> 
- 정상적인 재전송 큐 과정을 따르지 않고, 중간에 누락된 세그먼트를 빠르게 재전송

> 빠른 회복 fast recovery
> 
- 이미 여러번 ack가 온 경우, 세그먼트들이 순서가 어긋나게 수신이 되더라도, 네트워크 혼잡이라 여기지 않고 송신율을 빠르게 증가 시킴

---

> TCP 혼잡 제어 정책
> 

> TCP Tahoe
> 

처음에는 slow start를 사용하다가 임계점에 도달하면 AIMD 방식을 사용

그러다 3 ack duplicated 또는 타임아웃이 발생하면

임계점을 혼잡이 발생한 윈도우 크기의 절반으로 줄이고

윈도우 크기는 1로 줄인다.

![https://blog.kakaocdn.net/dn/cvqzii/btss9lnzXM0/YZNkJbSyWEVXcK3HrQzKa1/img.png](https://blog.kakaocdn.net/dn/cvqzii/btss9lnzXM0/YZNkJbSyWEVXcK3HrQzKa1/img.png)

> TCP Reno
> 

처음에는 slow start를 사용하다가 임계점에 도달하면 AIMD 방식을 사용

3 ack duplicated 와 타임아웃을 구분한다.

reno 는 3 ack duplicated 가 발생하면 빠른 회복 방식을 사용.

윈도우 크기를 반으로 줄이고 윈도우 크기를 선형적으로 증가시킨다.

임계점을 줄어든 윈도우 값으로 설정한다

![https://blog.kakaocdn.net/dn/cNvAqw/btssS06YxMA/8zgDiRRrsgUYkCkw3inT40/img.png](https://blog.kakaocdn.net/dn/cNvAqw/btssS06YxMA/8zgDiRRrsgUYkCkw3inT40/img.png)

|  | TCP Tahoe | TCP Reno |
| --- | --- | --- |
| 공통점 | 처음에는 slow start를 사용하다가임계점에 도달하면 AIMD 방식을 사용 | 처음에는 slow start를 사용하다가임계점에 도달하면 AIMD 방식을 사용 |
| 타임아웃과 3 ack 구분 | X | O |
| 윈도우 크기 | 1로 줄인다 | 반으로 줄인다 |
| 임계점 | 혼잡이 발생한 윈도우 크기의 절반 | 줄어든 윈도우 값 |

내용 및 이미지 출처

[http://www.ktword.co.kr/test/view/view.php?m_temp1=5536](http://www.ktword.co.kr/test/view/view.php?m_temp1=5536)

[](https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html)

[https://velog.io/@mu1616/TCPIP-%ED%98%BC%EC%9E%A1-%EC%A0%9C%EC%96%B4](https://velog.io/@mu1616/TCPIP-%ED%98%BC%EC%9E%A1-%EC%A0%9C%EC%96%B4)

[https://www.brianstorti.com/tcp-flow-control/](https://www.brianstorti.com/tcp-flow-control/)

[https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html](https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html)
