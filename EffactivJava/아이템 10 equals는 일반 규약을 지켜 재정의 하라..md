# 아이템 10 : equals는 일반 규약을 지켜 재정의 하라.

- equals 메서드는 재정의하기 쉬워 보이지만 곳곳에 함정이 도사리고 있어서 자칫하면 끔찍한 결과를 초래한다.
- 문제를 회피하는 가장 쉬운 길은 아예 재정의 하지 않는 것이다.

### 다음에서 열거한 상황 중 하나에 해당한다면 재정의 하지 않는 것이 최선이다.

- 각 인스턴스가 본질적으로 고유하다.
    - 값을 표현하는게 아니라 동작하는 개체를 표현하는 클래스일때
- 인스턴스’논리적 동치성(logical equality)’을 검사할 일이없다.
- 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
- 클래스가 private이거나 package-pravate이고 equals 메서드를 호출할 일이 없다.
    - 실수로라도 호출되는걸 막고싶다면 이렇게 구현해둔다.
    
    ```java
    @Override public boolean equals(Object o) {
    	throw new AssertionError(); // 호출 금지!
    }
    ```
    

### equals를 재정의 해야 할 때는 언제일까

- 객체 식별성이 아니라 논리적 동치성을 확인해야 하는데, 상위 클래스의 equals가 논리적 동치성을 비교하도록 재정의되지 않았을 때다.
    - Integer, String 처럼 값을 표현하는 클래스는, 두 값  의 논리적 동치성확인이 필요할것이다.
    - Enum의 경우에는 인스턴스가 둘이상 만들어지지 않기때문에 재정의 하지 않아도된다.
- equals 메서드를 재정의 할때는 반드시 일반 규약을 따라야 한다.
    - 반사성: null 이 아닌 모든 참조 값 x에 대해, x.equals(x)는 true다.
    - 대칭성: null 이 아닌 모든 참조 값, x,y 에 대해, x.equals(y)가 true면 y.equals(x)도 true다.
    - 추이성: null이 아닌 모든 참조 값,x,y,z에 대해 x.equals(y)가 true이고 y.equals(z)도 true면 x.equals(z)도 true다.
    - 일관성: null이 아닌 모든 참조 값 x,y에 대해, x.equals(y) 를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환한다.
    - null-아니 : null 이 아닌 모든 참조 값 x에 대해, x.equlas(null)은 false다.

### 구글이만든 AutoValue 를 활용하자