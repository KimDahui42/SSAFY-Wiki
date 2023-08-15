# 옵저버패턴

> 어떤 객체의 상태가 변할 때 그와 연관된 객체들에게 알림을 보내는 디자인 패턴
> 

: 객체의 상태 변화를 관찰하는 관찰자들, 즉, 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴임. 주로 분산 이벤트 핸들링 시스템을 구현하는데 사용됨. 발행/구독모델로 알려져 있기도 하다.

## 사용 시기

1. 앱이 한정된 시간, 특정한 케이스에만 다른 객체를 관찰해야 하는 경우
2. 대상 객체의 상태가 변경될 때마다 다른 객체의 동작을 트리거해야 할 때
3. 한 객체의 상태가 변경되면 다른 객체도 변경해야 할 때. 그런데 어떤 객체들이 변경되어야하는지 몰라도 될 때

## 장점

1. Subject의 상태 변경을 주기적으로 조회하지 않고 자동으로 감지 가능
2. 발행자의 코드를 변경하지 않고도 새 구독자 클래스를 도입할 수 있어서 OCP준수(개방폐쇄)
3. 런타임 시점에서에 발행자와 구독 알림 관계를 맺을 수 있음
4. 상태를 변경하는 객체와 변경을 감지하는 객체의 고나계를 느슨하게 유지가능

## 단점

1. 구독자는 알림 순서를 제어할 수 없고, 무작위 순서로 알림을 받음
>> 하드 코딩으론 구현가능하지만, 복잡성과 결합성만 높아짐 ㅠㅠ
2. 옵저버 패턴을 자주 구성하면 구조와 동작을 알아보기 힘들어져 복잡도 증가
3. 다수의 옵저버 객체를 등록 이후 해지하지 않는다면 메모리 누수 발생

## 예시

학생(크루)들은 코치가 하는 일들을 모두 알림 받아야한다! 즉, 코치가 “밥을 먹는다”하면 모든 크루들은 코치가 밥을 먹었다는 것을 알아야 한다. 코치가 “농땡이 친다”면 모든 크루들은 코치가 농땡이 치는 것을 알아야 한다.

베디 = 코치( 코치 인터페이스 구현해야함)
코치의 기능은 크루들을 등록한다, 크루들을 등록 해제한다, 크루들에게 행동을 알린다. 세가지 기능

![Untitled](%E1%84%8B%E1%85%A9%E1%86%B8%E1%84%8C%E1%85%A5%E1%84%87%E1%85%A5%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%200272ab0baffe4cbd9ebb263c83d94b0f/Untitled.png)

크루의 기능은 자신의 상태를 업데이트하는 기능을 가진다. 인터페이스를 정의해보자

```java
public interface Coach {
	void subscribe(Crew crew);
	void unsubscribe(Crew crew);
	void notifyCrew(String msg);
}

public interface Crew {
	void update(String msg);
}

// 두개의 인터페이스 정의했고, 코치를 구현하는 베디 클래스를 만들어보자
public class Baedi implements Coach {
	private List<Crew> crews = new ArrayList<>();
	public void eatFood() {
		System.out.println("베디코치가 밥을 먹는다.");
		notifyCrew("나 밥 먹ㅇ므");
	}
	public void runaway() {
		System.out.println("베디코치 농땡이침");
		notifyCrew("나 농ㅇ땡이침");
	}
	public void upgradeCutie() {
		System.out.println("베이코치가 귀여움을 강화");
		notifyCrew("나 더 귀여워 졌따");
	}
	@Override
	public void subscribe(Crew crew) {
		crews.add(crew);
	}
	@Override
	public void unsubscribe(Crew crew) {
		crews.remove(crew);
	}
	@Override
	public void notifyCrew(String msg) {
		crews.forEach(crew -> crew.update(msg));
	}

```

이렇게 코치는 crew들의 리스트를 가지고 있고, 세 가지 기능을 가진다.
그리고 인터페이스에 정의된 대로 3개의 함수를 구현한다. 주목할 것은 notifyCrew메서드를 각 기능에서 호출한다는 것임!!!! 그리고 크루들에게 한 명씩 업데이트 메서드를 호출하게 한다.(알림부분)
티버(크루)는 베디의 알림을 받고싶어서 구독을 하고 싶어한다.

![Untitled](%E1%84%8B%E1%85%A9%E1%86%B8%E1%84%8C%E1%85%A5%E1%84%87%E1%85%A5%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%200272ab0baffe4cbd9ebb263c83d94b0f/Untitled%201.png)

```java
public class Tiber implements Crew {
	@Override
	public void update(String msg) {
		System.out.println("Tiber 수신: " + msg);
	}
}
// 다른 크루들인 르윈, 제이도 마찬가지임. 코드는 생략할게요!!! 똑같음 이름만 달라!!
// 이제 메인 함수를 만들어봅시다.

public class Main {
	public static void main(String[] args) {
		Baedi baedi = new Baedi();
		Crew lewin = new Lewin();
		Crew tiber  new Tiber();
		Crew jay = new Jay();
		baedi.subscribe(lewin);
		baedi.subscribe(tiber);
		baedi.subscribe(jay);

		baedi.upgradeCutie();

// 코치인 베디에게 3명의 크루들이 구독을 했고, upgradeCutie()메서드를 호출했으니까 3명에게
// 메세지가 전달된다.
//베디코치가 귀여움을 강화했다
//Lewin 수신 : 나 더 귀여워 졌따
//Tiber 수신 : 나 더 귀여워 졌따
//Jay 수신 : 나 더 귀여워 졌따

// 근데 갑자기 lewin 이 빡쳐서 구독 해지함

		baedi.unsubscribe(lewin);
		baedi.eatFood();

//베디코치가 밥을 먹는다
//Tiber 수신 : 나 밥 먹ㅇ므
//Jay 수신 : 나 밥 먹ㅇ므

```

## 결론

이렇게 객체의 상태가 변화될 때 연관된 객체들에게 알림을 보낼 수 있게 된다. 이것이 옵저버 패턴.

조금더 옵저버 답게 이름을 리팩토링 하게 된다면?

Crew 인터페이스는 Observer인터페이스, Coach인터페이스는 Observable 인터페이스가 됨.

다시말해서!! Observer는 구독자!! Observable는 발행자!!!