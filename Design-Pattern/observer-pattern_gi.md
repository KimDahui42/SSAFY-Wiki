# observer pattern

태그: 디자인패턴
CS 주제: [3주차] 디자인패턴 (https://www.notion.so/3-d4384722e3ef47a3bfbb7de851960b49?pvs=21)
날짜: 2023년 8월 12일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: https://colab.research.google.com/github/NoCodeProgram/DesignPatterns/blob/main/Behavioral/observerP.ipynb

> 옵저버 패턴이란?
> 

관찰 중인 객체에 발생하는 모든 이벤트에 대해 알림을 받는 구독 메커니즘을 정의할 수 있도록 하는 패턴

> 분류
> 

행동 패턴

> 구조
> 

1. **출판사**는 다른 객체들에 관심 이벤트들을 발행한다. 이 이벤트들은 출판사가 상태를 전환하거나 어떤 행동을 실행할 때 발생.

출판사에는 구독 인프라가 포함되어 있으며, 이 인프라를 통해 현재 구독자들이 리스트를 떠나고 새 구독자들이 리스트에 가입

2. 새 이벤트가 발생하면, 출판사는 구독자 리스트를 살펴본 후 각 구독차 객체의 구독자 인터페이스에 선언된 알림 메서드를 호출.

3. 이 **구독자 인터페이스**는 알림 인터페이스를 선언하며, 대부분 경우 단일 update 메서드로 구성된다.

4. **구상 구독자**들은 출판사가 보낸 알림에 대한 응답으로 몇 가지 작업을 수행한다. 모든 클래스는 출판사가 구상 클래스들과 결합하지 않도록 같은 인터페이스를 구현해야 함

5. 출판사는 종종 콘텍스트 데이터를 알림 메서드의 인수로 전달한다. 출판사는 자신을 인수로 전달할 수 있으며, 구독자가 필요한 데이터를 직접 가져오도록 한다.

6. **클라이언트**는 출판사 및 구독자 객체를 별도로 생성한 후 구독자들을 출판사 업데이트에 등록한다.

> 장점
> 
- 개방/폐쇄 원칙. 출판사(publisher, subject, 발행자)의 코드를 변경하지 않고도 새 구독자 클래스를 도입할 수 있음. 그 반대로 출판사 인터페이스가 있는 경우 구독자 클래스를 변경하지 않고 새 출판사 클래스를 도입 가능.
- 런타임에 객체 간의 관계를 형성할 수 있다

> 단점
> 
- 구독자들은 무작위로 알림을 받는다

예시 코드

[https://colab.research.google.com/github/NoCodeProgram/DesignPatterns/blob/main/Behavioral/observerP.ipynb](https://colab.research.google.com/github/NoCodeProgram/DesignPatterns/blob/main/Behavioral/observerP.ipynb)