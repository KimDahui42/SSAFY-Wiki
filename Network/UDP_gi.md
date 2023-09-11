# UDP

태그: 네트워크
CS 주제: [6주차]네트워크 (https://www.notion.so/6-53e001231b7c404f9dbf2743e38a89ec?pvs=21)
날짜: 2023년 9월 4일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: https://www.geeksforgeeks.org/user-datagram-protocol-udp/, https://www.techtarget.com/searchnetworking/definition/UDP-User-Datagram-Protocol#:~:text=How%20UDP%20workspacket%20length%20and%20a%20checksum.
출석: 230903 출석 (https://www.notion.so/230903-7d1391f0e72e4635b462221abae04447?pvs=21)

> UDP 란?
> 

user datagram protocol

- 데이터를 데이터그램 단위로 처리하는 프로토콜
- **비연결형, 신뢰성 없는** 전송 프로토콜 ↔ TCP
- 전송계층에서 사용하는 프로토콜이다

> 특징
> 
- 데이터 전송에 앞서 연결을 구축할 필요가 없음
- low-latency
- loss-tolerating 커넥션에 사용 가능

> 언제 사용되는가
> 

TCP가 지배적인 전송 계층 프로토콜임

→ 그러나 추가적인 overhead (처리 시간, 메모리) 와 latency (지연)이 든다.

**실시간 서비스**(게이밍, 음성 및 영상 커뮤니케이션, 라이브 컨퍼런스) 에 **UDP**가 필요하다.

- 높은 수행도가 필요해서 패킷 전달이 지연되는 것보다도 drop하는 쪽을 허용한다
- 에러 확인 절차가 X
- **지연**과 **대역폭** 면에서 매우 **효율적**
- DNS와 NTP 등에서도 사용됨

> UDP header의 구성
> 

4개의 필드로 구성되어 있고 각각 2bytes

- source port number: sender의 수
- destination port number : 데이터그램이 전달되는 포트(의 숫자)
- length : udp 헤더와 캡슐화된 데이터의 길이( byte)
- checksum : 에러 체킹에 사용됨. IPv6에서는 필수, IPv4에서는 옵션

https://www.geeksforgeeks.org/user-datagram-protocol-udp/

[https://www.techtarget.com/searchnetworking/definition/UDP-User-Datagram-Protocol#:~:text=How%20UDP%20works,packet%20length%20and%20a%20checksum.](https://www.techtarget.com/searchnetworking/definition/UDP-User-Datagram-Protocol#:~:text=How%20UDP%20works,packet%20length%20and%20a%20checksum.)