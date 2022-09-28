# 아이템 12 : toString을 항상 재정의 하라

- 이메서드는 단순히 클래스_이름@16진수로_표시한_해시코드를 반환할 뿐이다.
- toString의 일반 규약에 따르면 ‘간결하면서 사람이 읽기 쉬운 형태의 유익한정보’ 를 반환해야 한다.
- toString의 규약은 “모든 하위 클래스에서 이메서드를 재정의하라” 라고 한다.
- toString을 잘 구현한 클래스는 사용하기에 훨씬 즐겁고, 그 클래스를 사용한 시스템은 디버깅하기 쉽다
- 실전에서 toString은 그 객체가 가진 주요 정보 모두를 반환하는 게 좋다.
    - 스스로를 완벽히 설명하는 문자열이어야 한다

포맷을 명시하든 아니든 여러분의 의도는 명확히 밝혀야한다.

```java
/*
* 이 전화번호의 문자열 표현을 반환한다.
* 이 문자열은 "XXX-YYY-ZZZZ" 형태의 12글자로 구성된다.
* XXX는 지역 코드, YYY는 프리픽스, ZZZZ는 가입자 번호다.
* 각각의 대문자는 10 진수 숫자 하나를 나타낸다.
*
* 전화번호의 각 부분의 값이 너무 작아서 자릿수를 채울 수 없다면, 
* 앞에서부터 0으로 채워나간다. 예컨대 가입자 번호가 123이라면
* 전화번호의 마지막 네 문자는 "0123"이 된다.
*
*/
@Override public String toString() {
	return  String.format("%03d-%03d-%04d",
													areaCode, prefix, lineNum);

}
```

포맷을 명시하지 않기로 했다면 다음처럼 작성할 수 있을것이다.

```java
/**
* 이 약물에 관한 대략적인 설명을 반환한다.
* 다음은 이 설명의 일반적인 형태이나,
* 상세 형식은 정해지지 않았으며 향후 변경될 수 있다.
*
* "[약물 #9: 유형 = 사랑 , 냄새 = 테레빈유, 겉모습 = 먹물]"
*/
@Override public String toString() {...}
```

- 정적 유틸리티 클래스(아이템 4) 는 toString을 제공할 이유가 없다.
- 또한 , 대부분의 열거 타입(아이템 34)도 자바가 이미 완벽한 toString을 제공하니 따로 재정의 하지 않아도 된다.
- 하지만 하위 클래스들이 공유해야 할 문자열 표현이있는 추상 클래스라면 toString을 재정의 해줘야 한다.

### AutoValue 프레임워크는 toString도 생성해 준다.

```java
@ToString
@EqualsAndHashCode
@Builder
@AllArgsConstructor(access = AccessLevel.PRIVATE)
public class User {
    private String name;
    private int age;
}
```