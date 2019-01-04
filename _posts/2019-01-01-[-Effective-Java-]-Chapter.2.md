# 2장 객체 생성과 파괴

- 객체를 만들어야 할 때와 만들지 말아야할 때를 구분
- 올바른 객체 생성 방법, 불필요한 생성을 피하는 방법





## Item 1.생성자 대신 정적 팩터리 메서드를 고려하라

- 정적 팩터리 메서드 : 클래스의 인스턴스를 반환하는 단순한 정적 메서드
- 클라이언트에 생성자 대신 정적 팩터리 메서드를 제공

#### 예시

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.True : Boolean.False;
}
```



### 장점

1. **이름을 가질 수 있다**. 정적 팩터리 메서드는 이름을 통해 객체의 특성을 묘사할 수 있다
   - 이름에 따라 여러가지 생성자를 제공하는 방식은 좋지 못하다.
2. **호출될 때마다 인스턴스를 새로 생성하지 않아도 된다**.
   - 이 덕분에 불변클래스는 인스턴스를 미리 만들거나 새로 생성한 인스턴스를 캐싱하여 **불필요한 객체 생성**을 막을 수 있다. 생성 비용이 큰 객체에 효과적.
     - 불변클래스 : 한번 생성되면 변경불가능한 클래스
   - 언제 어느 인스턴스를 살아 있게 할지 통제가 가능하다.
3. **반환 타입의 하위 타입 객체를 반환할 수 있다**. 이는 엄청난 유연성을 제공한다.
   - 구현 클래스를 공개하지 않고 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다.
4. **입력 매개변수에 따라 매번 다른 클래스를 객체를 반환할 수 있다.**
   - 반환 타입의 **하위 타입**이면 어떤 클래스의 객체든지 반환할 수 있다.
5. **정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.**
   - 이러한 유연함은 서비스 제공자 프레임워크를 만드는 근간이 된다.
   - 조건마다 구현체를 반환하는 유연성을 얻을 수 있다.



### 단점

1. **상속을 위해서 public이나 protected 생성성자가 필요하니 정적 팩터리 메서드 만으로 하위 클래스를 만들 수 없다.**
2. **정적 팩터리 메서드는 프로그래머가 찾기 어렵다.**
   - 생성자 처럼 API에 정확히 드러나지 않아 사용자가 찾기 어렵다.



### 자주 사용하는 명명 방식

#### 1.from : 매개변수 하나로 해당 타입의 인스턴스를 반환하는 형변환 메서드

```Java
Data d = Date.from(instant);
```

#### 2. of : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드

```java
Set<Rank> faceCards = EnumSet.of(Jack, Queen, King)
```





## Item 2. 생성자에 매개변수가 많다면 빌더를 고려하라.

- **문제점** : 정적 팩터리와 생성자는 **선택적 매개변수**가 많을 때 적절히 대응하기 어렵다.



#### 1. : 매개변수의 개수에 따라 생성자를 일일이 만드는 방식.

- 매개변수가 많아지면 코드를 작성하고 읽기 어렵다.

#### 2. : 매개변수가 없는 생성자로 객체를 생성한 후 setter메서드로 매개변수를 설정하는 방식.

- 객체 하나를 만들기 위해 여러 메서드를 호출해야 한다.
- 객체가 완전히 생성되기 전 일관성이 무너진 상태이다.
- 불변으로 만들 수 없다.

#### 3.

1. 클라이언트가 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자를 호출해 빌더 객체를 얻는다. 
2. 빌더 객체가 제공하는 setter 메서드로 매개변수를 설정한다.
3. 매개변수가 없는 build 메서드를 호출해 필요한(보통 불변인) 객체를 얻는다.

- 빌더 패턴 예시

```java
public class Car {
    private final int speed;
    private final int weight;
    private final int size;
    private final String wheel;
    private final String battery;
    
    
    public static class Builder {
        // 필수 매개변수
        private final int weight;
        private final int size;
        private final String battery;
        
        // 선택 매개변수
        private final int speed;
        private final String wheel;
        
        public Builder(int weight, int size, String battery) {
            this.weight = weight;
            this.size = size;
            this.battery = battery;
        }
        
        public Builder speed(int val) {
            this.speed = val;
            return this;
        }
        
        public Builder wheel(String val) {
            this.battery = val;
            return this;
        }
        
        public Car build() {
        	return new Car(this);
    	}
    }
    
    private Car(Builder builder) {
        weight = builder.weight;
        size = builder.size;
        //...
    }
}
```

- 빌더 배턴 실행 예시

```java
Car newCar = new Car.Builder(100, 200, "korean")
    				.speed(400)
    				.wheel("American")
```



- 빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기 좋다.
  - 각 계층의 클래스에 관련 빌더를 멤버로 정의하자. 추상 클래스는 추상 빌더를, 구체 클래스는 구체 빌더를 갖게 한다.





## Item 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라

- **싱글턴** : 인스턴스를 오직 하나만 생성할 수 있는 클래스
  - 커넥션 풀, 스레드 풀 같은 객체를 여러개 생성하면 자원적으로 낭비이기 때문에 하나만 생성하도록 한다.
- **문제점** : 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워 질 수 있다.



#### 싱글턴을 만드는 방법

### 1.생성자를 private으로 감추고, 인스턴스에 접근할 수단으로 public static 멤버를 생성한다.

```java
public class Car {
    public static final Car INSTANCE = new Car();
    private Car() {}
    //...
}
```

- 생성자는 한번만 호출된다. 
- Car 클래스가 전체 시스템에 하나 뿐임이 보장된다.
- 예외 : 리플렉션 API를 사용해 private 생성자를 호출할 수 있다.
  - 이를 방지하기 위해 두번째 객체가 생성되려 하면 예외를 던지도록 한다.

#### 장점 

1. 싱글턴임이 API에 명백히 드러난다. 간결하다.



### 2. 정적 팩터리 메서드를 public static 멤버로 제공한다.

```java
public class Car {
    private static final Car INSTANCE = new Car();
    private Car() {}
    public static Car getInstance() { return INSTANCE; }
}
```

- Car.getInstance()는 항상 **같은 객체의 참조를 반환**한다.
- 예외 : 리플렉션 API를 통한 접근은 여전히 가능하다.

#### 장점 

1. API를 변경하지 않고도 싱글턴이 아니게 할 수 있다.
2. 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있다.
3. 정적 팩터리의 메서드 참조를 공급자로 사용할 수 있다.
   - Car::getInstance => Supplier\<Car>



#### 직렬화

- 두 방식 모두 단순히 Serializable을 구현하는 것만으로는 안된다.
- 모든 인스턴스 필드를 **일시적(transient)**로 선언하고 **readResolve** 메서드를 제공해야 한다.
  - 안그러면 역직렬화할 때 마다 새로운 인스턴스가 만들어진다.

```java
private Object readResolve() {
    return INSTANCE;
}
```



### 3. 열거 타입

- 가장 좋은 방법
- 원소가 하나인 열거 타입을 선언
- 더욱 간결하고, 직렬화가 쉽다.
- 리플렉션 공격에도 안전하다.

```java
public enum Car {
    INSTANCE;
}
```





## Item 4. 인스턴스화를 막으려면 private 생성자를 사용하라

- 단순히 정적 필드만을 담은 클래스를 만들어야 할 때가 있다.
- 좋지는 않지만 나름 쓰임새가 있다.
  - java.lang.Math, java.util.Arrays
  - final 클래스와 관련된 메서드를 모아놓을 때
- **문제점** : 정적 멤버만 담은 클래스는 인스턴스로 만들어 쓰려고 설계한게 아니다.
- **추상클래스**
  - 인스턴스화를 막을 수 없다.
  - **하위 클래스를 만들어** 인스턴스화 할 수 있다.
- **private 생성자**를 추가하여 클래스의 인스턴스화를 막을 수 있다.

```java
public class utilityClass {
    private utilityClass() {
        throw new AssertionError();
    }
}
```

- 어떠한 환경에서도 인스턴스화를 막을 수 있다.
- 하지만, **상속이 불가능**하다.





## Item 5. 자원을 직접 명시하지 말고 의존객체 주입을 사용하라.

- 클래스는 하나 이상의 자원(클래스, 인터페이스, 관계...)에 의존한다.
- 맞춤법 검사기의 경우 사전(dictionary)에 의존한다.



### 1. 정적 유틸리티 클래스로 구현

```java
public class spellChecker {
    //사전
    private  static final Lexicon dictionary = ...;
    
    private spellChecker() {} //객체 생성 방지
}
```

- 유연하지 않고 테스트하기 어렵다.
  - 즉, 하나의 사전만 사용한다는 의미이다.



### 2. 싱글턴으로 구현

```java
public class spellChecker {
    //사전
    private  static final Lexicon dictionary = ...;
    
    private spellChecker(...) {}
    public static spellChecker INSTANCE = new spellChecker(...);
}
```

- 역시 유연하지 않고 테스트하기 어렵다.



- 사용하는 **자원에 따라 동작이 달라지는 클래스**에는 정적 유틸리티 클래스나 싱글턴 방식은 적합하지 않다.
- 인스턴스를 생성할 때 생성자에게 **필요한 자원을 넘겨주는 방식**으로 해결할 수 있다.
  - 클라이언트가 원하는 자원을 적용할 수 있다.



### 3. 의존 객체 주입

```java
public class spellChecker {
    private final Lexicon dictionary;
    
    //사전 자원을 인스턴스 생성시 넘겨준다
    public spellChecker(Lexicon dinctionary) {
        this.dictionary = Objects.requreNonNull(dictionary);
    }
}
```

- 자원에 상관없이 사용될 수 있다.
- **불변을 보장**하여 여러 클라이언트가 의존 객체를 안전하게 **공유**할 수 있다.

#### 변형 : 생성자에 자원 팩터리를 넘겨주는 방식

- 팩터리 : 호출할 때마다 특정 타입의 인스턴스를 **반복해서 만들어 주는 객체**



### 참고 자료

[의존 객체 주입](http://expert0226.tistory.com/195)





## Item 6. 불필요한 객체 생성을 피하라

- 똑같은 기능의 객체를 매번 생성하기 보다 객체 하나를 재사용 하는 편이 나을때가 많다.
  - 불변 객체는 언제든 재사용 할 수 있다.



#### 사용하면 안되는 극단적인 예

```java
String s = new String("Something")
```

- 실행할 때 마다 String 인스턴스를 새로 만들어 비효율적

#### 개선된 방식

```java
String s = "Something"
```

- 매번 새로운 인스턴스를 만드는게 아니라 하나의 String 인스턴스 사용
- 재사용이 보장된다.



- 정적 팩터리 메서드를 제공하여 불필요한 객체 생성을 피할 수 있다.
  - 팰터리 메서드는 호출할 때 마다 새로운 객체를 생성하지 않는다.
- 생성 비용이 비싼 객체는 캐싱해서 사용하는 것이 좋다.
  - 객체를 static으로 선언하여 캐싱이 가능하다.



### 오토 박싱

- 기본 타입과 박싱된 기본 타입을 섞어 쓸 때 상호 변환해주는 기술
- 불필요한 객체를 만들어 내는 대표적인 예
- 의도치 않은 오토 박싱이 발생하는지 주의하자

#### 예시

```java
private static long sum() {
    Long sum = 0L;	//프로그램을 느리게 만드는 원인
    for(long i = 0; i <= Integer.MAX_VALUE; i++) {
        sum += i;
    }
    
    return sum;
}
```

- **Long sum = 0L** 부분에서 Long으로 선언하여 불필요한 Long 인스턴스가 만들어 진다.



### 풀(Pool)

- 객체 생성 비용이 비싸 재사용하는 편이 낫다.
- 하지만, 객체 풀은 코드가 헷갈리고, 메모리 사용량을 너무 높여 성능을 떨어뜨린다.



#### '객체 생성은 비싸니 무조건 피해야 한다'가 아님을 명심하자





## Item 7. 다 쓴 객체 참조를 해제하라

- 가비지 컬렉터가 있다고 해서 메모리 관리에 신경을 쓰지 않아도 되는건 아니다.
- 메모리 누수가 발생할 수 있고 이는 프로그램 성능에 치명적일 수 있다.

#### 메모리 누수 예시

```java
public class Stack {
    private Object[] elements;
    private int size = 0;
    private stati final int INITIAL_CAPACITY = 16;
    
    public Stack() {
        elements = new Object[INITIAL_CAPACITY];
    }
    
    public void push(Object e) {
        //...
    }
    
    public Object pop() {
        if (size == 0) {
            //empty stack exception
        }
        
        return elements[--size];
    }
}
```

- pop() 부분에서 스택에서 꺼내진 객체를 가비지 컬렉터가 회수하지 않아 메모리 누수가 발생한다.
- 꺼내진 객체들은 더 이상 사용되지 않고 메모리 공간만 차지한다.
  - 스택이 객체들의 다 쓴 참조(obsolete reference)를 가지고 있다.

#### 메모리 누수 개선

```java
public Object pop() {
    if (size == 0) {
        //empty stack exception
    }
    Object result = elements[--size];
    elements[size] = null;
    
    return result
}
```

- 다 쓴 객체를 null 처리하여 메모리 누수 방지
- 또한, null 처리한 객체에 접근하려 하면 exception을 던져 더욱 안전하다.



- 하지만, 매번 null 처리 하는 것은 귀찮고, 코드가 지저분해진다.
- 따라서, 프로그래머가 항시 컬렉터가 찾지 못하는 메모리 누수에 주의해야 한다.



### 캐시

- 메모리 누수를 일으키는 주범
- 다 쓴 객체를 해제하여 메모리 누수를 방지할 수 있다.



### 리스너, 콜백

- 또한 메모리 누수를 일으키는 주범
- 클라이언트가 콜백을 등록만 하고 해제하지 않으면 문제가 발생한다.







