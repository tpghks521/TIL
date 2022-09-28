# 아이템 13: clone 재정의는 주의해서 진행하라

- Cloneable을 구현한 클래스의 인스턴세에서 clone을 호출하면 그 객체의 필드들을 하나하나 복사한 객체를 반환하며, 그렇지 않은 클래스의 인스턴스에서 호출하면 CloneNotSupportedExeption을 더ㄴ진다.
- 실무에서 Cloneable을 구현한 클래스 clone 메서드를 public 으로 제공하며, 사용자는 당연히 복제가 제대로 이뤄지리라 기대한다.
- 그결과 위험한 메커니즘이 탄생한다,
    - 생성자를 호출하지 않고도 객체를 생성할 수 있게 되는 것이다.

- clone 메서드는 사실상 생성자와 같은 효과를 낸다. 즉, clone은 원본 객체에 아무런 해를 끼치지 않는 동시에 복제된 객체의 불변식을 보장해야 한다.
    
    ```java
    @Override public Stack clone() {
    		try {
    			Stack result = (Stack) super.clone();
    			result.elements = elements.clone();
    			return result;
    		} catch (CloneNotSupportedException e) {
    				throw new AssertionError();
    		}
    }
    ```
    
    - elemets 또한 clone 을 진행해야 clone된 Stack이 같은 elements 인스턴스를 가지지않는다.
    - elements를 final로 선언한다면 불가능하다.
    - 위 이유 떄문에 Cloneable아키텍처는 ‘가변 객체를 참조하는 필드는 final로 선언하라’는 일반 용법과 충돌한다.
    
    ```java
    @Override public HashTable clone() {
    	try {
    		HashTable result = (HashTable) super.clone();
    		result.buckets = bueckest.clone();
    		resturn result;
     	} catch (CloneNotSupportedException e) {
    				throw new AssertionError();
    	}
    }
    ```
    
    - 링크드 리스트의 경우 복제본은 자신만의 버킷 배열을 갖지만, 이배열은 원본과 같은 연결리스트를 참조하여 원본과 복제본 모두 예기치 않게 동작할 가능성이 생긴다.

- 결론은 cloneable을 구현한 모든 클래스는 clone을 재정의 해야 한다.
    - 그말은 객체의 내부 ‘깊은 구조’ 에 숨어 있는 모든 가변 객체를 복사하고, 복제본이 가진 객체 참조 모두가 복사된 객체들을 가리키게 해야한다.
- 복사 생성자와 복사 펙터리라는 더 나은 객체 복사 방식을 제공할 수 있다.
    - 이게 베스트다.
    
    ```java
    public Yum(Yum yum) {...};
    
    public static Yum newInstance(Yum yum) { ... };
    ```
    
    Lombok을 사용한 clone 방법
    
    ```java
    @Builder(toBuilder = true)
    class Foo {
        // fields, etc
    }
    
    Foo foo = getReferenceToFooInstance();
    Foo copy = foo.toBuilder().build();
    ```
    
    tobuilder()를 테스트 해보자