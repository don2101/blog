# 5장. 제네릭

- 제네릭을 사용하면 컬렉션이 담을 수 있는 타입을 컴파일러에게 알려준다.
- 다만 코드가 복잡해질 수 있으므로 제네릭의 이점을 최대한 살리고 단점을 최소화 해야한다.



## 용어 정리

```java
매개변수화 타입 : List<String>
실제 타입 매개변수 : String
제네릭 타입 : List<E>
정규 타입 매개변수 : E
비 한정적 와일드카드 타입 : List<?>
로 타입 : List
한정적 타입 매개변수 : <E extends Number>
재귀적 타입 한정 : <T extends Comparable<T>>
한정적 와일드카드 타입 : List<? extends Number>
제네릭 메서드 : static<E> List<E> asList(E[] l)
타입 토큰 : String.class
```





## Item 26. 로 타입은 사용하지 마라

- 제네릭 클래스, 제네릭 인터페이스 : 클래스와 인터페이스 선언에 타입 매개변수 사용

```java
List<E>
```

- 통틀어 제네릭 타입 이라고 한다.
- 매개변수화 타입 : 클래스 이름 후 꺽쇠 괄호 안에 타입 매개변수 지정
- 로 타입(raw type) : 제네릭 타입에서 타입 매개변수를 전혀 사용하지 않을 때를 의미

```java
List<E>
```

- 위의 예 에서 로 타입은 List



#### 매개변수화된 컬렉션 타입 예

```java
private final Collection<Stamp> stamps = ...;
```

- stamp라는 변수에 Stamp 인스턴스만 넣어야 함을 컴파일러가 인지
- 다른 타입을 삽입할 경우 컴파일 오류 발생
- 매개변수 타입을 지정하지 않으면 컴파일러가 오류를 찾지 못하고 런타입 에러 발생



#### 예외 경우

```java
List<Object>
```

- 위의 예 처럼 임의 객체를 허용하는 매개변수화 타입은 괜찮다.

##### List 와 다른 점?

- List는 제네릭 타입과 전혀 상관 없으며, 위의 예는 임의 객체를 허용한다는 차이

- 또한 class 리터럴에는 로 타입을 써야 한다.



#### 비한정적 와일드카드 타입

- 제네릭 타입을 쓰고 싶지만 실제 타입 매개변수가 무엇인지 신경 쓰고 싶지 않을 때

```java
static in numbersIn(Set<?> s1) {...}
```

- 로 타입보다 안전하다. 로 타입은 아무 원소나 넣을 수 있으니 불변식 훼손할 가능성이 높다.
- 하지만 와일드카드 타입은 null 외에는 아무 것도 넣을 수 없다. 모종의 타입 객체만 저장할 수 있다.





## Item 27. 비검사 경고를 제거하라

- 제네릭을 사용할 때 마주하는 컴파일러 경고
  - 비검사 형변환 경고
  - 비검사 메서드 호출 경고
  - 비검사 매개변수화 가변인수 타입 경고
  - 비검사 변환 경고
- 하지만 비검사 경고는 쉽게 제거할 수 있다.

```java
// 안좋은 예
Set<smtn> exaltation = new HashSet();
// 올바른 예
Set<smtn> exalation = new HashSet<>();

```

- 위의 예에서 컴파일러가 올바른 타입 매개변수(smtn)을 적용한다.



- 할 수 있는한 보든 비검사 경고를 제거해야 한다.



- 한 줄이 넘는 메서드나 생성자에 달린 @SuppressWarnings annotation을 본다면 지역 변수 선언으로 옮기자.





## Item 28. 배열보다는 리스트를 사용하라

### (1) 배열 vs 리스트

- 배열 : 공변타입 - Sub가 Suber의 하위 타입이면 배열 Sub[]는 Super[]의 하위 타입
- 제네릭 : 불공변 - 서로 다른 타입

```java
Object[] objectArray = new Long[1];
objectArray[0] = "런타임 실패"
```

```java
List<Object> oList = new ArrayList<Long>();
oList.add("컴파일 에러")
```

- 리스트는 컴파일 시에 에러를 확인할 수 있음
- 배열은 실체화 된다.
  - 실체화 : 원소 타입을 컴파일 타임에만 검사하고 런타임에는 알 수가 없다.
- 제네릭 배열 사용 불가 이유 : 안전하지 않다.
- 제네릭 : 원소의 타입 정보가 소거되어 런타임에서는 무슨 타입인지 알 수 없다.
  - 따라서 배열 대신 리스트를 사용





## Item 29. 이왕이면 제네릭 타입으로 만들어라

### (1)일반 클래스를 제네릭 클래스로 만드는 방법

1. 클래스 선언에 타입 매개변수 추가

```java
//일반 클래스
public class Stack{
    private Object[] elements;
    //...
}

//제네릭 클래스
public class Stack<E> {
    private E[] elements;
}
```

#### 1.제네릭 클래스 생성시 오류 방지 방법

1. Object를 제네릭으로 변환

```java
public Stack() {
    elements = (E[]) new Object[size];
}
```

2. element 필드 타입을 E[]에서 Object[]로 변환





## Item 30. 이왕이면 제네릭 메서드로 만들어라

- 제네릭 메서드 작성법은 제네릭 타입 작성법과 비슷
- 재귀적 타입 한정 : 타입의 자연적 순서를 정하는 Comparable 인터페이스와 함께 쓰인다.





## Item 31. 한정적 와일드카드를 사용해 API 유연성을 높여라

- 매개변수화 타입은 불공변
  - List\<String>과 List\<Object>는 상하위 타입 관계가 아니다

에러가 발생하는, 와일드카드 타입을 사용하지 않은 메서드

```java
// 매개변수화 타입이 불공변이기 때문에 에러 발생

public void pushAll(Iterable<e> src) {
    for (E e : src)
        push(e)
}
```

- Iterable src의 원소 타입이 스택의 원소 타입과 다르면 오류 발생



E 생산자에 매개변수에 와일드 카드 타입 적용

```java
public void pushAll(Iterable<? extends E> src) {
    for (E e : src)
        push(e)
}
```

- pushAll의 입력 매개변수 타입은 'E의 iterable'이 아니라 'E의 하위 타입의 Iterable'이어야 한다.
- 와일드카드 타입 Iterable<? extend E>가 이를 지원한다.



- 유연성을 극대화하려면 원소의 생산자나 소비자용 입력 매개변수에 와일드카드 타입을 사용
- 하지만, 타입을 정확히 지정해야 하는 상황에는 와일드카드 타입을 쓰지 말아야 한다.



### 펙스(PECS)

- producer-extends, consumer-super
- 매개변수화 타입 T가 생산자라면 <? extend T>를 사용하고, 소비자라면 <? super t>를 사용



- 메서드 선언에 타입 매개변수가 한 번만 나오면 와일드 카드로 대체하라.











