# 아이템 4 : 인스턴스화를 막으려거든 private 생성자를 사용하라

- private 생성자를 추가하면 클래스의 인스턴스화를 막을 수 있다.

```java
public class UtilityClass {
// 기본 생성자가 만들어지는 것을 막는다(인스턴스화 방지용)/
	private UtilityClass() {
		throw new AssertionError();
	}
}

```

이코드는 어떤 환경에서도 클래스가 인스턴스화되는 것을 막아준다.

이 방식은 상속을 불가능하게 하는 효과도 있다.