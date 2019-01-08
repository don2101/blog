# 4장 클래스와 인터페이스

- 자바 언어에는 클래스와 인터페이스 설계에 사용하는 요소가 있다
- 이런 요소를 활용하여 클래스와 인터페이스를 쓰기 편하고, 견고하며, 유연하게 만드는 방법



## Item 15. 클래스와 멤버의 접근 권한을 최소화하라

- 잘 설계된 컴포넌트 : 클래스 내부 데이터와 구현 정보를 외부로 부터 얼마나 잘 숨겼느냐
  - 내부 구현을 숨겨, 구현과 API를 깔끔히 분리
- 오직 API를 통해서만 소통



### 정보 은닉의 장점

- 개발 속도 상승. 시스템 병렬개발 가능
- 시스템 관리 비용 감소. 컴포넌트 디버깅
- 성능 최적화에 도움
- 재사용성 증진
- 큰 시스템 제작 난이도 감소



### Java의 정보 은닉 장치

- 접근 제한자
- 기본 원칙 : 모든 클래스와 멤버의 접근성을 가능한 좁힌다.

#### 1. 클래스 접근 제한자

- public : 공개 API
- package-private : 패키지 안에서만 사용. 내부 구현되어 언제든 수정 가능. 
  클라이언트에 영향 없이 수정, 교체, 제거 가능.
- public일 필요가 없는 클래스의 접근 수준을 package-private 클래스로 좁혀라

#### 2. 멤버 접근 제한자

- private : 톱레벨 클래스에 접근
- package-private : 멤버가 소속된 패키지에서 접근
- protected : 상속받은 하위 클래스에서 접근
- public : 모든 곳에서 접근 가능



- 클래스의 공개 API를 살피고 모든 멤버는 private으로 선언하자
  - 필요한 곳만 package-private으로 풀 것
- 테스트시에는 private 멤버를 package-private 까지 풀어주는 것은 괜찮다
- public 클래스의 인스턴스 필드는 되도록 public이 아니어야 한다.
  - public 가변 필드는 일반적으로 스레드 안전하지도 않다.
- public static final : 클래스가 표현하는 추상 개념 완성에 필요한 상수일 경우 괜찮다.

#### 3. 배열

- 배열 : 길이가 0이 아닌 배열은 모두 변경 가능하니 주의할 것
  - 따라서, 클래스에서 public static final 배열 필드를 두거나 이 필드를 반환하는 접근 메서드를 제공해선 안된다.

```java
public static final objects[] VALUES = {...};
```

해결책

1. 앞 코드의 public 배열을 private으로 만들고 public 불변 리스트를 추가

```java
private static final objects[] PRIVATE_VALUES = {...};
public static final List<objects> VALUES = 
    Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
```

2. 배열을 private으로 만들고 그 복사본을 반환하는 public 메서드 추가

```java
private static final objects[] PRIVATE_VALUES = {...};
public static final objects[] values() { 
    return PRIVATE_VALUES.clone();
}
```





## Item 16. public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라

- 객체지향에서는 필드를 모두 private으로 만들고 접근자(setter)를 추가한다.
- public 클래스는 가변 필드를 노출해서는 안된다.
- 접근자를 통해 내부 표현 방식을 바꿀수 있는 유연성이 있다.
- private라면 데이터 필드를 노출해도 문제가 없다.
- public 클래스의 final 변수 또한 노출 단점은 줄지만 좋지는 않다.





## Item 17. 변경 가능성을 최소화 하라

-  불변 클래스 : 인스턴스 내부 값을 수정할 수 없는 클래스
  - 객체 파괴 순간까지 고정
  - String, BigInteger...
- 불변 클래스는 가변 클래스 보다 설계, 구현, 사용이 쉬우며 오류 확률이 적고 안전하다.



### 불변 클래스 규칙

- 객체 상태 변경 메서드를 제공하지 않는다.
- 클래스를 확장할 수 없도록 한다.(final 선언)
- 모든 필드는 final
- 모든 필드는 private
- 자신 외에 내부 가변 컴포넌트가 접근하지 못하게 한다.

```java
public final class Complex {
    private final real;
    private final imag;
    
    //constructor
    
    public Complex plus(Complex c) {
        return new Complex(real + c.real, imag + c.imag)
    }
    //methods...
}
```

- 메서드 또한 새로운 인스턴스를 반환하도록 한다.



### 장점

- 불변 객체는 생성 시점의 상태를 파괴 순간 까지 유지한다.
- 불변 객체는 근본적으로 스레드 안전하므로 따로 동기화 할 필요가 없다.
  - 안심하고 공유가 가능한다.
- 불변 객체는 자유롭게 공유할 수 있으며, 불변 객체끼리 내부 데이터를 공유할 수 있다.
- 객체를 만들 때 불변 객체를 구성요소로 사용하면 이점이 많다.
  - 집합으로 사용할 수 있음
- 불변 객체는 그 자체로 실패 원자성을 제공한다.
  - 실패 원자성 : 예외 처리 이후에도 똑같은(유효한) 상태임을 보장



### 단점

- 값이 다르면 새로운 객체를 만들어야 한다.



### 불변 클래스 설계 방법

- 자기 자신을 상속하지 못하게 하라 : 생성자를 private으로 만들고 public 정적 팩터리 제공

```java
private Complex(...) {
    //...
}

public static Complex valueOf(double real, double imag) {
    return new Complex(real, imag)
}
```





