# class diagram

태그: 디자인패턴, 소프트웨어공학
CS 주제: [1주차] 디자인패턴 (https://www.notion.so/1-f1717caf520f4af7aecf1150103f5e7a?pvs=21)
날짜: 2023년 7월 30일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: https://en.wikipedia.org/wiki/Unified_Modeling_Language, https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-class-diagram/
출석: 230730 출석 (https://www.notion.so/230730-937cbc74cc354eb1a0a1617b3e003054?pvs=21)

> UML : unified modeling language
> 

시스템 디자인의 시각화에 있어 표준의 방법을 제공하기 위한 일반적 목적의 시각적 모델링 언어다.

[Unified Modeling Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language)

> 클래스 다이어그램이란?
> 

UML 내의 클래스 다이어그램은 정적 구조 다이어그램의 한 종류다.

시스템 내의 클래스, 속성, 메소드와 객체간의 관계를 보여줌으로써 시스템의 구조를 describe한다.

![https://blog.kakaocdn.net/dn/bOnMZA/btspsWk3hSH/FoDZuINbR4IKv84tgPfulk/img.png](https://blog.kakaocdn.net/dn/bOnMZA/btspsWk3hSH/FoDZuINbR4IKv84tgPfulk/img.png)

출처: https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-class-diagram/

> 클래스 다이어그램의 목적
> 

1. 시스템 classifiers의 정적 구조를 보여준다

2. UML에 의해 미리 정해진 다른 구조에 대한 기본적인 지식을 제공

3. 개발자와 다른 팀 멤버들에게 도움이 된다

> 클래스 다이어그램의 구성
> 
- classes 의 세트와
- classes 간의 관계의 세트로 구성된다

> 클래스란?
> 

시스템 내에서 유사한 역할을 하는 객체들의 모음

structural features(속성) + behavioral features(메서드)

더 자세한 건 이 글에 정리해둠

[클래스, 객체, 인스턴스, 메서드 | 변수와 타입의 관계](https://timedilation.tistory.com/46)

[](https://timedilation.tistory.com/46)

> 클래스 간 관계 유형
> 

1. 상속 **inheritance**

- an "is-a" relationship

![https://blog.kakaocdn.net/dn/pVlCg/btspxbhUD9M/DRwUBOrwwBV5hiLynckBe0/img.png](https://blog.kakaocdn.net/dn/pVlCg/btspxbhUD9M/DRwUBOrwwBV5hiLynckBe0/img.png)

2. 단순 연관 **simple association**

- 두 동료 클래스의 구조적 링크

![https://blog.kakaocdn.net/dn/XKFJq/btspxcum17f/kdHU62Dffd6Ltc4SpWGkSK/img.png](https://blog.kakaocdn.net/dn/XKFJq/btspxcum17f/kdHU62Dffd6Ltc4SpWGkSK/img.png)

3. 집합 **aggregation**

- 속해 있음
- 클래스2가 클래스1에 포함되어 있음
- 두 클래스의 생애주기는 분리되어있음

![https://blog.kakaocdn.net/dn/Y1poF/btsplOhfLAT/UkU3TX9c2iwWTctuRNKde0/img.png](https://blog.kakaocdn.net/dn/Y1poF/btsplOhfLAT/UkU3TX9c2iwWTctuRNKde0/img.png)

4. 구성 **composition**

- 집합 관계 중에서, 전체가 파괴되면 부분도 파괴되는 유형
- 클래스2는 클래스1 없이 존재 불가능

![https://blog.kakaocdn.net/dn/dcOwHL/btspmLdzF4x/7aAcXR7ZabmE3N8pdmXk60/img.png](https://blog.kakaocdn.net/dn/dcOwHL/btspmLdzF4x/7aAcXR7ZabmE3N8pdmXk60/img.png)

5. 의존 **dependency**

- 하나의 정의가 바뀌면 다른 쪽의 정의도 바뀔수 있음 (반대쪽으로는 아님)
- 클래스1은 클래스2에 의존한다

![https://blog.kakaocdn.net/dn/bgzPw0/btspsFDzu81/RmlZzGZYk5w4Kk3WkiYLck/img.png](https://blog.kakaocdn.net/dn/bgzPw0/btspsFDzu81/RmlZzGZYk5w4Kk3WkiYLck/img.png)

> 관계 이름
> 
- 연결 선 중간에 이름이 적혀있음

> 역할
> 
- 역할은 연결의 직접적인 **목적(용도)**임
- 역할은 연결 선의 끝에 적혀있고, 관계 내에서의 해당 클래스가 수행하는 **목적(용도)**을 설명한다.

> navigability 탐색가능성
> 
- 화살표는 한 관계에 있는 인스턴스가 관련된 다른 클래스의 인스턴스들을 determine할 수 있는가를 보여준다.

예를 들어... 스프레드 시트 → 셀 탐색은 가능한데 셀 → 스프레드 시트 탐색은 불가능하다

> 클래스 속성과 오퍼레이션의 가시성 visibility
> 

1. public **+**

2. protected **#**

3. private **-**

4. package **~**

의 네 종류가 있음.

클래스 내의 속성과 오퍼레이션 앞에 + - # ~ 기호로 표시한다.

> 권한 범위
> 

| 접근 권한 | public + | private - | protected # | package ~ |
| --- | --- | --- | --- | --- |
| 같은 클래스의 멤버 | O | O | O | O |
| 파생 클래스의 멤버 | O | X | O | O |
| 다른 클래스의 멤버 | O | X | X | 같은 패키지 내에서만 |

내용 및 모든 사진 출처

[What is Class Diagram?](https://www.visual-paradigm.com/guide/uml-unified-modeling-language/what-is-class-diagram/)