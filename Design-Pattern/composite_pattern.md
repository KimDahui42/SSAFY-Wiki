# 복합체 패턴

태그: 디자인패턴
CS 주제: [4주차] 디자인패턴 (https://www.notion.so/4-4e7285f063184a688a62e90db438a676?pvs=21)
날짜: 2023년 8월 19일
블로그 게시: No
완료: Yes
작성자: 박주헌
참고자료: https://gmlwjd9405.github.io/2018/08/10/composite-pattern.html, https://refactoring.guru/ko/design-patterns/composite
출석: 230819 출석 (https://www.notion.so/230819-c06369c172604d68b9ab25bd42ee3e2a?pvs=21)

# 복합체 패턴(Composite)

---

### What?

- **복합체 패턴이란**
    
    **복합체** 패턴은 객체들을 트리 구조들로 구성한 후, 이러한 구조들과 개별 객체들처럼 작업할 수 있도록 하는 구조 패턴.
    

---

### Why?

- **복합체 패턴을 쓰는 이유**
    - **나무와 같은 객체 구조를 구현해야 할 때.**
        - 복합체 패턴은 공통 인터페이스를 공유하는 두 가지 기본 요소 유형들인 단순 잎들과 복합 컨테이너들을 제공한다. 컨테이너는 잎들과 다른 컨테이너들로 구성될 수 있으며, 이를 통해 나무와 유사한 중첩된 재귀 객체 구조를 구성할 수 있다.
    
    - **클라이언트 코드가 단순 요소들과 복합 요소들을 모두 균일하게 처리하도록 하고 싶을 때 사용.**
        - 복합체 패턴에 의해 정의된 모든 요소들은 공통 인터페이스를 공유하며, 이 인터페이스를 사용하면 클라이언트는 작업하는 객체들의 구상 클래스에 대해 걱정할 필요가 없다.

---

### How?

- **복합체 패턴의 사용방법**
    
    ```python
    // 컴포넌트 인터페이스는 합성 관계의 단순 객체와 복잡한 객체 모두를 위한 공통
    // 작업들을 선언합니다.
    interface Graphic is
        method move(x, y)
        method draw()
    
    // 잎 클래스는 합성 관계의 최종 객체들을 나타냅니다. 잎 객체는 하위 객체들을 가질
    // 수 없습니다. 일반적으로 실제 작업을 수행하는 것은 잎 객체들이며, 복합체 객체들은
    // 오로지 자신의 하위 컴포넌트에만 작업을 위임합니다.
    class Dot implements Graphic is
        field x, y
    
        constructor Dot(x, y) { ... }
    
        method move(x, y) is
            this.x += x, this.y += y
    
        method draw() is
            // X와 Y에 점을 그립니다.
    
    // 모든 컴포넌트 클래스들은 다른 컴포넌트들을 확장할 수 있습니다.
    class Circle extends Dot is
        field radius
    
        constructor Circle(x, y, radius) { ... }
    
        method draw() is
            // X와 Y에 반지름이 R인 원을 그립니다.
    
    // 복합체 클래스는 자식이 있을 수 있는 복잡한 컴포넌트들을 나타냅니다. 복합체
    // 객체들은 일반적으로 실제 작업을 자식들에 위임한 다음 결과를 '합산'합니다.
    class CompoundGraphic implements Graphic is
        field children: array of Graphic
    
        // 복합체 객체는 자식 리스트에 단순한 또는 복잡한 다른 컴포넌트들을 추가하거나
        // 제거할 수 있습니다.
        method add(child: Graphic) is
            // 하나의 자식을 자식들의 배열에 추가합니다.
    
        method remove(child: Graphic) is
            // 하나의 자식을 자식들의 배열에서 제거합니다.
    
        method move(x, y) is
            foreach (child in children) do
                child.move(x, y)
    
        // 복합체는 특정 방식으로 기본 논리를 실행합니다. 복합체는 모든 자식을
        // 재귀적으로 순회하여 결과들을 수집하고 요약합니다. 복합체의 자식들이 이러한
        // 호출들을 자신의 자식들 등으로 전달하기 때문에 결과적으로 전체 객체 트리를
        // 순회하게 됩니다.
        method draw() is
            // 1. 각 자식 컴포넌트에 대해:
            //     - 컴포넌트를 그리세요.
            //     - 경계 사각형을 업데이트하세요.
            // 2. 경계 좌표를 사용하여 점선 직사각형을 그리세요.
    
    // 클라이언트 코드는 기초 인터페이스를 통해 모든 컴포넌트와 함께 작동합니다. 그래야
    // 클라이언트 코드가 단순한 잎 컴포넌트들과 복잡한 복합체들을 지원할 수 있습니다.
    class ImageEditor is
        field all: CompoundGraphic
    
        method load() is
            all = new CompoundGraphic()
            all.add(new Dot(1, 2))
            all.add(new Circle(5, 3, 10))
            // …
    
        // 선택한 컴포넌트들을 하나의 복잡한 복합체 컴포넌트로 합성합니다.
        method groupSelected(components: array of Graphic) is
            group = new CompoundGraphic()
            foreach (component in components) do
                group.add(component)
                all.remove(component)
            all.add(group)
            // 모든 컴포넌트가 그려질 것입니다.
            all.draw()
    ```
    

---