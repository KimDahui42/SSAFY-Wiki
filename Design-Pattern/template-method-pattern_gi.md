# template method pattern

태그: 디자인패턴
CS 주제: [3주차] 디자인패턴 (https://www.notion.so/3-d4384722e3ef47a3bfbb7de851960b49?pvs=21)
날짜: 2023년 8월 14일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html, https://refactoring.guru/ko/design-patterns/template-method
출석: 230820 출석 (https://www.notion.so/230820-3f13f8f5127442c1ac3e1162e94e22e4?pvs=21)

> 템플릿 메소드 패턴이란?
> 

부모 클래스에서 알고리즘의 골격을 정의하지만, 해당 알고리즘의 구조를 변경하지 않고

**자식 클래스**들이 알고리즘의 특정 단계들을 **오버라이드(재정의)**할 수 있게 하는 행동 디자인 패턴

> 분류
> 

행동 패턴

> 왜 쓰는가?
> 
- 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 **코드 중복을 최소화**할 때 유용
- 동일한 **기능을 상위 클래스에서 정의**하면서 확장, **변화가 필요한 부분만 서브 클래스**에서 구현할 수 있음

> 단계
> 
- 모든 자식 클래스는 추상 단계를 구현해야 한다
- 선택적 단계에는 이미 어떤 디폴트 구현이 있지만, 필요한 경우 이를 무시하고 오버라이드(재정의)할 수 있음

> 훅
> 

훅은 몸체가 비어있는 선택적 단계.

템플릿 메서드는 훅이 오버라이드 되지 않아도 작동한다.

일반적으로 훅은 알고리즘의 중요한 단계들 전후에 배치되어 자식 클래스의 알고리즘에 대한 추가 확장 지점 제공

> 구조
> 

![https://blog.kakaocdn.net/dn/nvWwN/btsq0Fca761/RmXHiwUZtZKDLZvJmPKT51/img.png](https://blog.kakaocdn.net/dn/nvWwN/btsq0Fca761/RmXHiwUZtZKDLZvJmPKT51/img.png)

리팩토링 구루 https://refactoring.guru/ko/design-patterns/template-method

1. **추상 클래스**는 알고리즘 단계들의 역할을 하는 메서드를 선언하며, 이러한 메서드를 특정 순서로 호출하는 실제 템플릿 메서드도 선언한다. 단계들은 abstract 로 선언되거나 일부 디폴트 구현을 갖는다

2. **구상 클래스**는 모든 단계들을 오버라이드할 수  있지만 템플릿 메서드 자체는 오버라이드할 수 없음.

> 장점
> 
- 클라이언트가 대규모 알고리즘의 특정 부분만 오버라이드 하도록 해서, 다른 부분에 발생하는 변경에 영향을 덜 받도록 함
- 중복 코드를 부모 클래스로 가져올 수 있음

> 단점
> 
- 일부 클라이언트는 알고리즘의 제공된 골격에 제한됨
- 자식 클래스를 통한 디폴트 단계 구현을 억제하여 **리스코프 치환 원칙 위반 가능성**
- 단계가 많을수록 유지가 더 어려운 경향이 있음

내용 참고 및 출처

[https://refactoring.guru/ko/design-patterns/template-method](https://refactoring.guru/ko/design-patterns/template-method)

[https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html](https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html)