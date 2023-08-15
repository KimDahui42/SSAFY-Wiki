# 어댑터 패턴

‘Wrapper 패턴’

## 정의

 이미 만들어진 것을 그대로 사용할 수 없을 때, 필요한 형태로 사용할 수 있게 해주는 패턴이다. 이미 만들어 진 것을 감싸는 형태여서 Wrapper 패턴이라고도 한다. ~~변환기라고 생각하면 될듯?!~~ 기존에 이미 잘 구축되어 있는 것을 새로운 어떤 것이 사용할 때 양쪽 간의 호환성을 유지해주기 위함이다. 자바에서도 직접적으로 메서드는 호출하지 않고 중간에 어댑터를 거쳐서 메서드를 호출하도록 하는 패턴이다.

![Untitled](%E1%84%8B%E1%85%A5%E1%84%83%E1%85%A2%E1%86%B8%E1%84%90%E1%85%A5%20%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB%2063b58eec09f147b789c2c206c5db5b78/Untitled.png)

## 패턴 사용 시기

1. 레거시 코드를 사용하고 싶지만 새로운 인터페이스가 레거시 코드와 호환되지 않을 때
2. 이미 만든 것을 재사용하고자 하나 이 재사용 가능한 라이브러리를 수정할 수 없을 때
3. 이미 만들어진 클래스를 새로운 인터페이스에 맞게 개조할 때
4. 소프트웨어의 구 버전과 신 버전을 공존시키고 싶을 때

## 장점

1. SRP만족
- 프로그램의 기본 비즈니스 로직에서 인터페이스 또는 데이터 변환 코드를 분리할 수 있음
2. OCP만족
- 기존 클래스 코드를 건들지 않고 클라이언트 인터페이스를 통해 어댑터와 작동하기 때문
3. 버그가 발생해도 기존의 클래스에는 버그가 없으므로 Adapter역할의 클래스를 중점적으로 조사하면 되고, 프로그램 검사도 쉬워짐

## 단점

1. 새로운 인터페이스와 어댑터 클래스 세트를 도입해야 하기 때문에 코드의 복잡성 증가
2. 때로는 직접 서비스클래스를 변경하는 것이 간단할 수 있음
3. 

## 구현

```jsx
public interface ArmCore {
		public abstract void up();
		public abstract void down();
}

public class Arm implements ArmCore {
		public Arm() {
				System.out.println("Make Robot Arm");
		}
		
		@Override
		public void up() {
				System.out.println("Robot Arm Up");
		}
		
		@Override
		public void down() {
				System.out.println("Robot Arm Down");
		}
}

public class RobotSystem {
		private ArmCore rightArm;
		private ArmCore leftArm;

		public RobotSystem (Armcore rightArm, ArmCore leftArm) {
				this.rightArm = rightArm;
				this.leftArm = leftArm;
		}
	
		public void armUp() [
				rightArm.up();
				leftArm.up();
		}
	
		public void armDown() {
				rightArm.down();
				leftArm.down();
		}

//main
RobotSystem robot = new RobotSystem(new Arm(), new Arm());

robot.armDown();
robot.armUp();
```

여기서 왼쪽 팔을 신제품으로 교체하되, 신제품의 소스는 수정하지 말라는 요청이 들어온다면?

```jsx
//신제품 팔
public class NewArm {
		public NewArm() {
				System.out.println("Make New Robot Arm");
		}
	
		public void lift() {
				System.out.println("New Robot Arm lift");
		}
		
		public void fall() {
				System.out.println("New Robot Arm fall");
		}
}

//신제품 팔 어댑터
public class NewArmAdapter extends NewArm implements ArmCore {
		public NewArmAdapter() {}

		@Override
		public void up() {
				super.lift();
		}

		@Override
		public void down() {
				super.fall(); 
		}
}

//여기서 왼팔에 신제품 소스를 붙여주기만 하면 끝!

//MAIN
RobotSystem robot = new RobotSystem(new Arm(), new NewArmAdapter());
robot.armDown();
robot.armUp();
```