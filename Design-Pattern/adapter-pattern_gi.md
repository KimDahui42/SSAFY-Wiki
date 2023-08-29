# adapter pattern

태그: 디자인패턴
CS 주제:  [2주차] 디자인패턴 (https://www.notion.so/2-a262137b3eba4db79de885eca3e85e38?pvs=21)
날짜: 2023년 8월 6일
블로그 게시: Yes
완료: Yes
작성자: 김지연
참고자료: https://refactoring.guru/
출석: 230807 출석 (https://www.notion.so/230807-e69f8e83c06c4b238938f08f419c73c7?pvs=21)

> 어댑터 패턴  (a.k.a Wrapper)
> 

incompatible interfaces를 collaborate 가능하게 하는 structural design pattern이다.

다른 객체가 이해할 수 있도록 한 객체의 인터페이스를 변환해주는 특별한 객체다.

복잡도 ☆

사용빈도 ☆☆☆

> 언제 사용할까?
> 

클래스를 바로 사용할 수 없을 때, 중간에서 **변환 역할을 해주는 클래스**가 필요하다.

예를 들어, 내 앱은 json 형태의 데이터를 사용하는데, 내가 받아오려는 데이터는 xml형식일 때

있는 그대로 받아올 수는 없음.

> 어떻게 동작하는가?
> 

bts에서 발생하는 변환의 복잡성을 가려주는 객체 중 하나(추상화)

the wrapped object는 이 어댑터를 인식조차 할 수 없음.

1. 어댑터는 이미 있는 객체들 중 하나와 compatible한 인터페이스를 하나 갖는다

2. 이 인터페이스를 사용하여, 이미 있던 객체가 안전하게 어댑터의 메소드를 호출할 수 있다

3. 호출을 받으면, 어댑터는 두번째 객체에게 요청을 pass한다. 요청 형식은 두번째 객체가 예상하는 방식이다.

> 구조
> 

1. **클라이언트**는 기존의 프로그램 로직을 포함하고 있는 클래스임

2. **클라이언트 인터페이스**는 다른 클래스들이 협업을 하려면 반드시 따라야 하는 프로토콜을 describe

3. **서비스**는 어떠한 기능을 가진 클래스임. 클라이언트는 이 클래스를 바로 쓸 수는 없다. 인터페이스가 다르기 때문에.

4. **어댑터**는 클라이언트와 서비스 모두와 동작 가능하다.

클라이언트 인터페이스를 implements(구현)하는 동시에 서비스 객체를 랩핑하고 있다.

어댑터는 어댑터 인터페이스를 통해 클라이언트로부터 호출을 받고, 그것을 wrapped service 객체에 번역해 전달한다.

5. 클라이언트 코드는 클라이언트 인터페이스로 동작하는 이상 어댑터와 **커플링 되지 않는다.**

그러므로 기존의 클라이언트 코드를 깨뜨리지 않고 새로운 타입의 어댑터들을 가지고 올수 있다.

서비스 클래스의 인터페이스가 변경되었을 때 유용하다. 클라이언트 코드를 바꾸지 않고 새로운 어댑터만 만들면 되기 때문.

> 예시코드 (파이썬)
> 

```python
class Target:
    """
    클라이언트 코드가 사용하는 특정 도메인 인터페이스를 정의함
    """
    def request(self) -> str:
        return "Target: 디폴트 타겟의 행동."

class Adaptee:
    """
    어댑티 (서비스)는 유용한 기능을 가지고 있지만,
    인터페이스가 기존 클라이언트 코드와 incompatible
    어댑테이션이 필요한 상태
    """
    def specific_request(self) -> str:
        return "동행 한별특 의티댑어"

class Adapter(Target, Adaptee):
    """
    어댑터는 어댑티의 다중 상속을 통해
    어댑티 인터페이스가 타겟 인터페이스와 양립 가능하도록 만듦
    """

    def request(self) -> str:
        return f"Adapter: (TRANSLATED) {self.specific_request()[::-1]}"

def client_code(target: "Target"):
    """
    클라이언트 코드는 타겟 인터페이스를 따르는 모든 클래스를 지원함
    """
    print(target.request(), end="")

if __name__ == "__main__":
    print("Client: 나는 타겟 객체와 원활히 일할 수 있어:")
    target = Target()
    client_code(target)
    print("\n")

    adaptee = Adaptee()
    print("Client: 어댑티 코드는 이상한 인터페이스를 쓴다"
          "봐 내가 못알아듣잖아:")
    print(f"Adaptee: {adaptee.specific_request()}", end="\n\n")

    print("Client: 하지만 어댑터를 통해 같이 일할 수 있어:")
    adapter = Adapter()
    client_code(adapter)
```

 

output

```python
Client: 나는 타겟 객체와 원활히 일할 수 있어:
Target: 디폴트 타겟의 행동.

Client: 어댑티 코드는 이상한 인터페이스를 쓴다봐 내가 못알아듣잖아:
Adaptee: 동행 한별특 의티댑어

Client: 하지만 어댑터를 통해 같이 일할 수 있어:
Adapter: (TRANSLATED) 어댑티의 특별한 행동
```

내용 및 코드예제 

[https://refactoring.guru/](https://refactoring.guru/)