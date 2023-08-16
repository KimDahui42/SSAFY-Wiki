# 싱글톤 패턴

‘객체의 인스턴스를 한 개만 생성되게 하는 패턴’

---

---

## 정의

 소프트웨어 디자인 패턴에서 싱글턴 패턴을 따르는 클래스는 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고, 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다. 주로 공통된 객체를 여러개 생성해서 사용하는 DBCP(DataBase Connection Pool)와 같은 상황에서 많이 사용된다.

## 장점

1. 메모리 측면의 이점
싱글톤 패턴을 사용하게 되면 한 개의 인스턴스만을 고정 메모리 영역에 생성하고, 추후 해당 객체를 접근할 때 메모리 낭비를 방지할 수 있다.
2. 속도 측면의 이점
생성된 인스턴스를 사용할 때는 이미 생성된 인스턴스를 활용하여 속도 측면에 이점이 있다.
3. 데이터 공유가 쉬움
전역으로 사용하는 인스턴스이기 때문에 다른 여러 클래스에서 데이터를 공유하며 사용할 수 있다. 하지만 동시성 문제*가 발생할 수 있어 이 점은 유의하여 설계해야 한다.
    
      *동시성 문제: **동일한 하나의 데이터에 2 이상의 스레드, 혹은 세션에서 가변 데이터를 동시에 제어할 때 나타는 문제**로, 하나의 세션이 데이터를 수정 중일때, 다른 세션에서 수정 전의 데이터를 조회해 로직을 처리함으로써 데이터의 정합성(데이터들 값이 서로 일치하는 상태)이 깨지는 문제
    

## 구현

```java
public class Singleton {
    // 단 1개만 존재해야 하는 객체의 인스턴스*로 static 으로 선언
		// static: 미리 선언한다!!
    private static Singleton instance;

    // private 생성자로 외부에서 객체 생성을 막아야 한다.
    private Singleton() {
    }

    // 외부에서는 getInstance() 로 instance 를 반환
    public static Singleton getInstance() {
        // instance 가 null 일 때만 생성
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

// 같은 instance인지 Test
public class Application {
    public static void main(String[] args) {
        Singleton singleton1 = Singleton.getInstance();
        Singleton singleton2 = Singleton.getInstance();

        System.out.println(singleton1);
        System.out.println(singleton2);
    }
}
        /** Output
         * vendingmachine.Singleton@15db9742
         * vendingmachine.Singleton@15db9742
         **/

싱글톤 객체를 생성하는 위의 코드를 실행해보면 두 객체가 하나의 인스턴스를 사용하여 같은 주소 값을 출력하는 것을 확인할 수 있습니다.
```

*인스턴스: 객체 지향 프로그래밍에서 인스턴스는 해당 클래스의 구조로 컴퓨터 저장공간에서 할당된 실체를 의미

## 문제점

1. 싱글턴 인스턴스를 여러 곳에서 많이 공유할 경우, 다른 클래스의 인스턴스 간 의존성이 높아짐 
>> 객체지향 원칙에 어긋남.
2. 의존성이 높아진다면 수정 작업이나 테스트를 진행하기 어려워짐
3. 멀티스레드 환경에서 객체가 1개이상 생성되어 오류 발생의 여지가 있음