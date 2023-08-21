# 전략 패턴

태그: 디자인패턴
CS 주제: [4주차] 디자인패턴 (https://www.notion.so/4-4e7285f063184a688a62e90db438a676?pvs=21)
날짜: 2023년 8월 19일
블로그 게시: No
완료: Yes
작성자: 박주헌
참고자료: https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html, https://refactoring.guru/ko/design-patterns/strategy
출석: 230819 출석 (https://www.notion.so/230819-c06369c172604d68b9ab25bd42ee3e2a?pvs=21)

# 전략 패턴(Strategy)

---

### What?

- 전략 **패턴이란**
    
    전략 패턴은 알고리즘들의 패밀리를 정의하고, 각 패밀리를 별도의 클래스에 넣은 후 그들의 객체들을 상호교환할 수 있도록 하는 행동 디자인 패턴.
    

---

### Why?

- **전략 패턴을 쓰는 이유**
    - **객체 내에서 한 알고리즘의 다양한 변형들을 사용하고 싶을 때, 그리고 런타임 중에 한 알고리즘에서 다른 알고리즘으로 전환하고 싶을 때.**
        - 전략 패턴은 객체의 행동들을 특정 하위 행동들을 다양한 방식으로 수행할 수 있는 다른 하위 객체들과 연관시켜 객체의 행동들을 런타임에 간접적으로 변경할 수 있게 해준다.
    
    - **일부 행동을 실행하는 방식에서만 차이가 있는 유사한 클래스들이 많은 경우.**
        - 전략 패턴은 다양한 행동들을 별도의 클래스 계층구조로 추출하고 원래 클래스들을 하나로 결합하므로 중복코드를 제거할 수 있다.
    
    - **클래스의 비즈니스 로직을 해당 로직의 콘텍스트에서 그리 중요하지 않을지도 모르는 알고리즘들의 구현 세부 사항들로부터 고립시키고 싶을 때.**
        - 전략 패턴은 코드의 나머지 부분에서 해당 코드, 내부 데이터, 그리고 다양한 알고리즘들의 의존 관계들을 고립시킬 수 있다.  따라서, 다양한 클라이언트들이 알고리즘들을 실행하고 런타임에 전환하기 위한 간단한 인터페이스를 가질 수 있음.
    
    - **같은 알고리즘의 다른 변형들 사이를 전환하는 거대한 조건문이 클래스에 있는 경우.**
        - 전략 패턴을 사용하면 모든 알고리즘을 같은 인터페이스를 구현하는 별도의 클래스들로 추출하여 이러한 조건문을 제거할 수 있다. 원래 객체는 알고리즘의 모든 변형들을 구현하는 대신 이러한 객체들 중 하나에 실행을 위임함.

---

### How?

- 전략 **패턴의 사용방법**
    
    ```python
    // 전략 인터페이스는 어떤 알고리즘의 모든 지원 버전에 공통적인 작업을 선언합니다.
    // 콘텍스트는 이 인터페이스를 사용하여 구상 전략들에 의해 정의된 알고리즘을
    // 호출합니다.
    interface Strategy is
        method execute(a, b)
    
    // 구상 전략들은 기초 전략 인터페이스를 따르면서 알고리즘을 구현합니다. 인터페이스는
    // 그들이 콘텍스트에서 상호교환할 수 있게 만듭니다.
    class ConcreteStrategyAdd implements Strategy is
        method execute(a, b) is
            return a + b
    
    class ConcreteStrategySubtract implements Strategy is
        method execute(a, b) is
            return a - b
    
    class ConcreteStrategyMultiply implements Strategy is
        method execute(a, b) is
            return a * b
    
    // 콘텍스트는 클라이언트들이 관심을 갖는 인터페이스를 정의합니다.
    class Context is
        // 콘텍스트는 전략 객체 중 하나에 대한 참조를 유지합니다. 콘텍스트는 전략의
        // 구상 클래스를 알지 못하며, 전략 인터페이스를 통해 모든 전략과 함께
        // 작동해야 합니다.
        private strategy: Strategy
    
        // 일반적으로 콘텍스트는 생성자를 통해 전략을 수락하고 런타임에 전략이 전환될
        // 수 있도록 세터도 제공합니다.
        method setStrategy(Strategy strategy) is
            this.strategy = strategy
    
        // 콘텍스트는 자체적으로 여러 버전의 알고리즘을 구현하는 대신 일부 작업을 전략
        // 객체에 위임합니다.
        method executeStrategy(int a, int b) is
            return strategy.execute(a, b)
    
    // 클라이언트 코드는 구상 전략을 선택하고 콘텍스트에 전달합니다. 클라이언트는 올바른
    // 선택을 하기 위해 전략 간의 차이점을 알고 있어야 합니다.
    class ExampleApplication is
        method main() is
            Create context object.
    
            Read first number.
            Read last number.
            Read the desired action from user input.
    
            if (action == addition) then
                context.setStrategy(new ConcreteStrategyAdd())
    
            if (action == subtraction) then
                context.setStrategy(new ConcreteStrategySubtract())
    
            if (action == multiplication) then
                context.setStrategy(new ConcreteStrategyMultiply())
    
            result = context.executeStrategy(First number, Second number)
    
            Print result.
    ```
    

---

- SOLID 전략의 모든 특징을 가지고 있음