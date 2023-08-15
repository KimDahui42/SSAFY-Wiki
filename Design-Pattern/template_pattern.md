# 템플릿 메서드 패턴

태그: 디자인패턴
CS 주제: [3주차] 디자인패턴 (https://www.notion.so/3-d4384722e3ef47a3bfbb7de851960b49?pvs=21)
날짜: 2023년 8월 12일
블로그 게시: No
완료: No
작성자: 박주헌
참고자료: https://gmlwjd9405.github.io/2018/07/13/template-method-pattern.html, https://refactoring.guru/ko/design-patterns/template-method
출석: 230812 출석 (https://www.notion.so/230812-ac15e50bb8cb413c97ea7b5708bd5477?pvs=21)

# 템플릿 메서드 패턴(Template Method)

---

### What?

- **템플릿 메서드 패턴**
    
    **템플릿 메서드**는 부모 클래스에서 알고리즘의 골격을 정의하지만, 해당 알고리즘의 구조를 변경하지 않고 자식 클래스들이 알고리즘의 특정 단계들을 오버라이드(재정의)할 수 있도록 하는 행동 디자인 패턴.
    

---

### Why?

- **템플릿 메서드 패턴을 쓰는 이유**
    - 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화 할 때 유용하다.
    - 다른 관점에서 보면 동일한 기능을 상위 클래스에서 정의하면서 확장/변화가 필요한 부분만 서브 클래스에서 구현할 수 있다.
    - 예를 들어, 전체적인 알고리즘은 상위 클래스에서 구현하면서 다른 부분은 하위 클래스에서 구현할 수 있도록 함으로써 전체적인 알고리즘 코드를 재사용하는 데 유용하다.

---

### How?

- **템플릿 메서드 패턴의 사용방법**
    
    ```abap
    // 추상 클래스는 템플릿 메서드를 정의합니다. 이 메서드는 일반적으로 원시 작업을
    // 추상화하기 위해 호출로 구성된 어떤 알고리즘의 골격을 포함합니다. 구상 자식
    // 클래스들은 이러한 작업을 구현하지만 템플릿 메서드 자체는 그대로 둡니다.
    class GameAI is
        // 템플릿 메서드는 알고리즘의 골격을 정의합니다.
        method turn() is
            collectResources()
            buildStructures()
            buildUnits()
            attack()
    
        // 일부 단계들은 기초 클래스에서 바로 구현될 수 있습니다.
        method collectResources() is
            foreach (s in this.builtStructures) do
                s.collect()
    
        // 그리고 그중 일부는 추상으로 정의될 수 있습니다.
        abstract method buildStructures()
        abstract method buildUnits()
    
        // 한 클래스에는 여러 템플릿 메서드가 있을 수 있습니다.
        method attack() is
            enemy = closestEnemy()
            if (enemy == null)
                sendScouts(map.center)
            else
                sendWarriors(enemy.position)
    
        abstract method sendScouts(position)
        abstract method sendWarriors(position)
    
    // 구상 클래스들은 기초 클래스의 모든 추상 작업을 구현해야 합니다. 하지만 템플릿
    // 메서드 자체를 오버라이드해서는 안 됩니다.
    class OrcsAI extends GameAI is
        method buildStructures() is
            if (there are some resources) then
                // 농장들, 막사들, 그리고 요새들을 차례로 건설하세요.
    
        method buildUnits() is
            if (there are plenty of resources) then
                if (there are no scouts)
                    // 잡역인을 생성한 후 정찰병 그룹에 추가하세요.
                else
                    // 하급 병사를 생성한 후 전사 그룹에 추가하세요.
    
        // …
    
        method sendScouts(position) is
            if (scouts.length > 0) then
                // 정찰병들을 위치로 보내세요.
    
        method sendWarriors(position) is
            if (warriors.length > 5) then
                // 전사들을 위치로 보내세요.
    
    // 자식 클래스들은 디폴트 구현을 가진 일부 작업을 오버라이드할 수 있습니다.
    class MonstersAI extends GameAI is
        method collectResources() is
            // 몬스터들은 자원을 모으지 않습니다.
    
        method buildStructures() is
            // 몬스터들은 건물을 짓지 않습니다.
    
        method buildUnits() is
            // 몬스터들은 유닛들을 생성하지 않습니다.
    ```
    

---