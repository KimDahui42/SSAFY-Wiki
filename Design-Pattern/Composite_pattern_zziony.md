# 컴포지트 패턴(Composite Pattern)

> 복합 객체(Composite) 와 단일 객체(Leaf)를 동일한 컴포넌트로 취급하여, 클라이언트에게 이 둘을 구분하지 않고 동일한 인터페이스를 사용하도록 하는 구조 패턴
> 

: 복합체 패턴은 전체-부분의 관계를 갖는 객체들 사이의 관계를 트리 계층 구조로 정의해야 할 때 유용하게 사용. 윈도우나 리눅수의 파일 시스템 구조를 떠올려보면 쉽게 이해가능!

폴더(디렉토리)안에는 파일이 들어 있을 수도 있고, 파일을 담은 또 다른 폴더도 들어있을 수 있다. 이를 복합적으로 담을 수 있다 해서 Composite객체라고 불린다. 반면 파일은 단일 객체이기 때문에 Leaf객체라고 불리운다. 즉 Leaf는 자식이 없다!

![Untitled](../src/images/compostie_zziony1.png)

복합체 패턴은 이런 폴더와 파일을 동일한 타입으로 취급하여 구현을 단순화 시키는 것이 목적이다. 폴더 안에는 파일 뿐 아니라 서브 폴더가 올 수 있고, 또 서브 폴더 안에도 서브 폴더가 있을 수 있으니.. 이런식으로 계층 구조를 구현하다 보면 복잡해 질 수 있는 복합객체를 **재귀 동작**을 통해 하위 객체들에게 작업을 위임한다. 그러면 복합 객체와 단일 객체를 대상으로 똑같은 작업을 적용할 수 있어 단일 / 복합 객체를 구분할 필요가 거의 없어진다.

<aside>
💡 정리하자면 Composite패턴은 그릇과 내용물을 동일시해서 재귀적인 구조를 만들기 위한 디자인 패턴!!

</aside>

## 사용 시기

- 데이터를 다룰 때 계층적 트리 표현을 다루어야 할 때
- 복잡하고 난해한 단일 / 복합 객체 관계를 간편히 단순화하여 균일하게 처리하고 싶을 때

## 장점

- 단일체와 복합체를 동일하게 여기기 때문에 묶어서 연산하거나 관리할 때 편함
- 다형성 재귀를 통해 복잡한 트리 구조를 보다 편리하게 구성할 수 있음
- 수평적, 수직적 모든 방향으로 객체를 확장 가능
- 새로운 Leaf 클래스를 추가 하더라도 클라이언트는 추상화된 인터페이스 만을 바라보기 때문에 OCP준수. (단일 부분의 확장이 용이)

## 단점

- 재귀 호출 특징 상 트리의 깊이(depth)가 깊어질 수록 디버깅에 어려움이 생김
- 설계가 지나치게 범용성을 갖기 때문에 새로운 요소를 추가할 때 복합 객체에서 구성 요소에 제약을 갖기 힘듦
- 예를 들어서, 계층형 구조에서 leaf객체와 composite객체들을 모두 동일한 인터페이스로 다루어야하는데, 이 공통 인터페이스 설계가 까다로움
- 복합 객체가 가지는 부분 객체의 종류를 제한할 필요가 있을 때
- 수평적 방향으로만 확장이 가능하도록 Leaf를 제한하는 Composite를 만들때

## 예제!!

<아이템을 담는 가방을 담은 가방>

복합객체와 단일 객체를 상자와 아이템으로 비유하자면 아래 그림과 같이 표현할 수 있습니다. 상자 안에 아이템을 넣고, 다시 그 상자를 상자 안에 넣는 식입니다.

![Untitled](../src/images/compostie_zziony2.png)

이러한 구조를 객체지향 프로그래밍으로 보다 유지보수가 용이하게 컴포지트 패턴을 통해 구현해봅시다!

Item클래스와, 이를 담는 Bag 클래스가 있다고 하자. 가방 안에 아이템을 담는 형식이니 Item 클래스는 Leaf가 되고, Bag 클래스는 Compostie가 된다.

우리가 구현하고 싶은 것은 bag 속 리스트에 담겨진 item 객체들의 총 가격값을 추출하고 싶다. 그런데 단순히 가방 안에 아이템들이 들어있을 뿐 아니라 복수의 아이템을 담은 또 다른 가방들이 여러개 들어있을 수 있고 그 가방안에 가방이 들어있을 수 있따. 그렇다면 루트 가방일 경우 하위 계층까지 순회하여 아이템의 가격을 합산해야 하는데, 이러한 계층 트리 구조를 컴포지트 패턴으로 클래스를 구성하면 아래와 같다

1. composite와 leaf객체를 공용으로 묶는 ItemComponent 인터페이스를 정의하고, compostie와 leaf객체에서 동시에 쓰이는 추상메소드를 정의
2. composite 객체인 bag 클래스에서 ItemComponent타입의 공용 아이템을 담는 내부 리스트를 정의
3. Component인터페이스의 공통적인 operation 인 getPrice()메서드는 Item일 경우 그대로 price값을 반환하면 되지만, bag일 경우 가방은 단순히 아이템을 담는 그릇일 뿐이기 때문에 자기 자신을 호출하여 현재 가방에 들어있는 Item을 순회하는 재귀 동작을 행하게 된다

![Untitled](../src/images/compostie_zziony3.png)

```java
//Component 인터페이스
interface ItemComponent {
	int getPrice();
	String getName();
}

//Composite 객체
class Bag implements ItemComponent {
	//아이템들과 서브 가방 모두를 저장하기 위해 인터페이스 리스트로 관리
List<ItemComponent> components = new ArrayList<>();

String name; // 가방 이름

public Bag(String name) {
	this.name = name;
}

//리스트에 아이템, 가방 추가
public void add(ItemComponent item) {
	components.add(item);
}

//현재 가방의 내용물 반환
public List<ItemComponent> getComponents() {
	return components;
}

@Override
public int getPrice() {
	int sum = 0;
	for (ItemComponent component : components) {
		//만일 리스트에서 가져온 요소가 Item이면 정수값을 받고
		// Bag이면 '재귀 함수' 동작이 되게 함
		sum += component.getPrice(); // 자기 자신 호출(재귀)
	}
	return sum; // 재귀적으로 돌아 하위 아이템들의 값을 더하고 반환
}

@Override
public String getName() {
	return name;
}
```

```java
//Leaf
class Item implements ItemComponent {
	String name;
	int price;
	
	public Item(String name, int price) {
		this.name = name;
		this.price = price;
	}
	@Override
	public int getPrice() {
		return price;
	}
	@Override
	public String getName() {
		return name;
	}
}
```

```java
class Client {
	public static void main(String[] args) {
		//1. 메인 가방 인스턴스 생성
		Bag bag_main = new Bag("메인 가방");

		//2. 아이템인스턴스 생성
		Item armor = new Item("armor", 250);
		Item sword = new Item("sw", 500);
		//3. 메인 가방에는 모험에 필요한 무구 아이템만을 추가
		bag_main.add(armor);
		bag_main.add(sword);
		//4. 서브 가방 인스턴스 생성
		Bag bag_food = new Bag("음식 가방");
		// 5. 아이템 인스턴스 생성
		Item apple = new Item("apple", 400);
		Item banana = new Item("bananaL", 130);

		//6. 서브 가방에는 음식 아이템만을 추가
		bag_food.add(apple);
		bag_food.add(banana);
		//7. 서브 가방을 메인 가방에 넣음
		bag_main.add(bag_food);

		Client client = new Client();

		//가방안에 있는 모든 아이템의 총 값어치를 출력
		//가방 안에 아이템 뿐아니라 서브 가방도 있음
		client.printPrice(bag_main);

		// 서브 가방안에 있는 모든 아이템의 총 값어치를 출력
		client.printPrice(bag_food);
	}

	public void printPrice(ItemCompoent bag) {
		int result = bag.getPrice();
		System.out.println(bag.getName() + "의 아이템 총합: " + result + " 골드");
	}
}

//출력!!
// 메인 가방의 아이템 총합 : 1280 골드
// 음식 가방의 아이템 총합 : 530 골드
```

클라이언트에서는 ItemComponent만을 사용하기 때문에 Item, Bag 구현체 상관 없이(전체, 개별 구분 없이) 구현 메소드만 호출하면 내부에서 정의된 구현에 따라 원하는 값을 얻을 수 있게 된다.