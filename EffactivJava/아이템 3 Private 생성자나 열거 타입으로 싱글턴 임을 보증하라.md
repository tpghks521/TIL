# 아이템 3 : Private 생성자나 열거 타입으로 싱글턴임을 보증하라

- 싱글턴 이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다.

```java
public class Elvis {
	public static final Elvis INSTANCE = new Elvis();
	private Elvis(){...}
	
	public void leaveTheBuilding(){ ... }
}
```

- pirvate 생성자는 public static final 필드인 Elvis.INSTANCE를 초기화할때 딱 한 번만 호출된다.
- 인스턴스가 전체 시스템에서 하나뿐임을 보장할수있다.

### 싱글턴을 만드는 두번째 방법에서는 정적 팩터리메서드를 public static 멤버로 제공한다.

```java
public class Elvis {
	private static final Elivs INSTANCE = new Elvis(
	private Elvis() {...}
	public static Elvis getInstnace() {return INSTANCE;}
	public void levaeTheBuilding() {...}
}
```

Elvis.getInstances는 항상 같은 객체의 참조를 반환한다.

- 정적팩터리 방식의 장점은 싱글턴이 아닌 방식으로도 얼마든지 변경이가능하다.
- 원한다면 정적 팩터리를 제네릭 싱글턴 패턴으로 바꿀수도있다.

### 싱글턴을 만드는 세 번쨰 방법은 원소가 하나인 열거 타입을 선언하는 것이다.

 

```java
public enum Elivs{
	INSTANCE;
	
	public void leaveTheBuilding() {...}
}
```

대부분 상황에서는 원소가 하나분인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다.