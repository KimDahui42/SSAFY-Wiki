# TCP 3 & 4way handshake

태그: 네트워크
CS 주제: [5주차] 네트워크 (https://www.notion.so/5-8a44a8a690a248a2a237f9ff12ec3b49?pvs=21)
날짜: 2023년 8월 28일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake
출석: 230828 출석 (https://www.notion.so/230828-2e0b14db5675446da3cbfd861fa8344b?pvs=21)

> TCP 3 way handshake 란
> 
- TCP 는 정확한 전송을 보장한다는 특징이 있음.
- 그래서 데이터 전송 이전에 먼저 장치들 사이에 논리적인 접속을 성립하기 위해 three way handshake를 사용한다.

> 역할
> 
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장
- 양쪽 모두 상대편에 대한 초기 순차일련번호를 얻을 수 있도록 한다

> 과정
> 

> step 1 - SYN_SENT
> 

클라이언트는 서버에 접속을 요청하는 SYN 패킷을 보낸다

이때  클라이언트는 SYN / ACK 응답을 기다리는 SYN_SENT 상태가 됨

> step 2 - SYN_RECEIVED
> 

서버는 syn을 요청받고 클라이언트에게 요청을 수락한다는 ACK 와 SYN flag 가 설정된 패킷을 발송한다.

그리고 클라이언트가 다시 ACK로 응답하기를 기다림

이때 서버는 SYN_RECEIVED 상태가 됨

> step 3 - ESTABLISHED
> 

클라이언트는 서버에게 ACK 를 보낸다

이후로는 연결이 이루어지고 데이터가 오간다.

이때 서버의 상태가 ESTABLISHED 이다.

---

> 4 way handshake 란?
> 
- 세션을 종료하기 위해 수행하는 절차.

↔ 3way는 연결을 (초기화)할 때 사용함

> 과정
> 

> step 1 - FIN flag
> 

클라이언트가 연결을 종료하겠다는 **FIN flag**를 전송한다

> step 2 - TIME_WAIT
> 

서버는 확인메시지를 보내고 자신의 통신이 끝날때까지 기다린다.

이 상태를 time_wait 상태라 한다.

> step 3 - FIN flag
> 

서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 **FIN 플래그**를 전송한다

> step 4
> 

클라이언트는 확인했다는 메시지를 보낸다.

※ 클라이언트에게도 time_wait 과정이 있음

→ 만약 서버에서 fin을 전송하기 전에 전송한 패킷이 라우팅 지연이나 패킷 유실로 인한 재전송으로 인해 fin 패킷보다 늦게 도착하는 상황이 발생한다면?

→ 이런 상황에서 데이터 유실에 대비하여 클라이언트는 서버로부터 fin을 수신하더라도 일정시간 (디폴트 240초) 동안 세션을 남겨 놓고 잉여 패킷을 기다리는 과정을 거친다.

이는 step 4까지 모두 끝난 뒤에 일어나는 과정임

내용출처

[https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake](https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake)