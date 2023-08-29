# composite pattern

태그: 디자인패턴
CS 주제: [4주차] 디자인패턴 (https://www.notion.so/4-4e7285f063184a688a62e90db438a676?pvs=21)
날짜: 2023년 8월 21일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: https://refactoring.guru/ko/design-patterns/composite
출석: 230821 출석 (https://www.notion.so/230821-1bb30c53e4b44a18ae1d1f992f553708?pvs=21)

복합체 패턴, 객체 트리 등으로도 불린다.

> 컴포지트 패턴이란?
> 

객체들을 트리 구조로 구성한 후, 이 구조들과 개별 객체들처럼 작업할 수 있도록 하는 구조 패턴이다

> 작동 방식
> 
- 메서드를 호출하면 **객체들 자체**가 요청을 트리 아래로 전달한다
- 복합체 패턴은 객체 트리의 모든 컴포넌트들에 대해 **재귀적**으로 행동을 실행할 수 있게 한다

> 구조
> 

![https://blog.kakaocdn.net/dn/PLTIF/btsrH4ulBt4/LwWaK5KUr4xMxEV4LGDxWk/img.png](https://blog.kakaocdn.net/dn/PLTIF/btsrH4ulBt4/LwWaK5KUr4xMxEV4LGDxWk/img.png)

출처: 리팩토링구루

1. **컴포넌트 인터페이스**는 트리의 단순 요소들과 복잡한 요소들 모두에 **공통적인 작업을 설명**한다.

2. 잎은 트리의 **베이스** 요소이며, 하위 요소가 없다. - 작업을 위임 x 실제 작업 **수행**

3. 컨테이너 (복합체)는 하위 요소가 있는 요소다. 자녀들의 구상 클래스를 알지 못하며, 컴포넌트 인터페이스를 통해서만 하위요소들과 함께 작동.

→ 요청을 전달받아 작업을 하위 요소에 위임, 중간 결과 처리, 최종 결과 반환.

> 장점
> 
- **다형성과 재귀**를 활용해 복잡한 트리 구조와 더 편리하게 작업 가능
- **개방/폐쇄 원칙**을 따른다. 객체 트리와 작동하는 기존 코드를 훼손하지 않고 새로운 유형 요소를 도입

> 단점
> 
- 적용이 어려운 경우가 발생한다. 컴포넌트 인터페이스를 **과도하게 일반화**하는 경우 이해가 어렵다

내용 출처

[https://refactoring.guru/ko/design-patterns/composite](https://refactoring.guru/ko/design-patterns/composite)