# 아이템 11: equals를 재정의하려거든 hashCode도 재정의 하라

### equals를 재정의한 클래스 모두에서 hashCode도 재정의 해야한다.

- equals 비교에 사용되는 정보가 변경되지 않았다면, 애플리케이션이 실행되는 동안 그객체의 hashCode 메서드는 몇 번을 호출해도 일관되게 항상 같은 값을 반환해야 한다. (단, 애플리케이션을 다시 실행한다면 이 값이 달라져도 상관없다.
- equals(Object)가 두 객체를 같다고 판단했다면, 두객체의 hashCode는 똑같은 값을 반환해야 한다.
- equals(Object)가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 단, 다른 객체에 대해서는 다른 값을 반환해야 해시 테이블의 성능이 좋아진다.
    - 논리적으로 같다고 판단될수있지만 ,Object의 기본 hashCode 메서드는 이둘이 다르다고판단할 경우

### 논리적 동치성을 가지는 객체의 해시코드 메서드

```java
@Override public int hashCode() {
	int result = Short.hashCode(areaCode);
	result = 31 * result + Short.hashCode(prefix);
	result = 31 * result + Short.hashCode(lineNum);
	return result;
}
```

- 인스턴스의 핵심 필드 3개를 사용해 계산 한다.
- 과정에서 비결정적(undeterministic) 요소는 전혀 없으므로 논리적 동치를 가지는 인스턴스들은 같은 해시코드를 가질것이 확실해진다.
- 구아바 같은 라이브러리 활용또한 가능하다.

### AutoValue를 사용하면 HashCode도 자동으로 만들어준다.