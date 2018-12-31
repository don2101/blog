# 2장 객체 생성과 파괴

- 객체를 만들어야 할 때와 만들지 말아야할 때를 구분
- 올바른 객체 생성 방법, 불필요한 생성을 피하는 방법





## Item 1.생성자 대신 정적 팩터리 메서드를 고려하라

- 정적 팩터리 메서드 : 클래스의 인스턴스를 반환하는 단순한 정적 메서드
- 클라이언트에 생성자 대신 정적 팩터리 메서드를 제공

##### 예시

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.True : Boolean.False;
}
```



#### 장점

1. **이름을 가질 수 있다**. 정적 팩터리 메서드는 이름을 통해 객체의 특성을 묘사할 수 있다
   - 이름에 따라 여러가지 생성자를 제공하는 방식은 좋지 못하다.
2. **호출될 때마다 인스턴스를 새로 생성하지 않아도 된다**.
   - 이 덕분에 불변클래스는 인스턴스를 미리 만들거나 새로 생성한 인스턴스를 캐싱하여 **불필요한 객체 생성**을 막을 수 있다. 생성 비용이 큰 객체에 효과적.
     - 불변클래스 : 한번 생성되면 변경불가능한 클래스
   - 언제 어느 인스턴스를 살아 있게 할지 통제가 가능하다.
3. **반환 타입의 하위 타입 객체를 반환할 수 있다**. 이는 엄청난 유연성을 제
   - 구현 클래스를 공개하지 않고 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다.
4. **입력 매개변수에 따라 매번 다른 클래스를 객체를 반환할 수 있다.**
   - 반환 타입의 **하위 타입**이면 어떤 클래스의 객체든지 반환할 수 있다.
5. **정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.**
   - 이러한 유연함은 서비스 제공자 프레임워크를 만드는 근간이 된다.
   - 조건마다 구현체를 반환하는 유연성을 얻을 수 있다.



#### 단점

1. **상속을 위해서 public이나 protected 생성성자가 필요하니 정적 팩터리 메서드 만으로 하위 클래스를 만들 수 없다.**
2. **정적 팩터리 메서드는 프로그래머가 찾기 어렵다.**
   - 생성자 처럼 API에 정확히 드러나지 않아 사용자가 찾기 어렵다.



#### 자주 사용하는 명명 방식

1. **from** : 매개변수 하나로 해당 타입의 인스턴스를 반환하는 형변환 메서드

```Java
Data d = Date.from(instant);
```

1. **of** : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드

```java
Set<Rank> faceCards = EnumSet.of(Jack, Queen, King)
```





## Item 2. 생성자에 매개변수가 많다면 빌더를 고려하라.

- 정적 팩터리와 생성자는 **선택적 매개변수**가 많을 때 적절히 대응하기 어렵다.



1. **점층적 생성자 패턴** : 매개변수의 개수에 따라 생성자를 일일이 만드는 방식.
   - 매개변수가 많아지면 코드를 작성하고 읽기 어렵다.
2. **자바 빈즈 패턴** : 매개변수가 없는 생성자로 객체를 생성한 후 setter메서드로 매개변수를 설정하는 방식.
   - 객체 하나를 만들기 위해 여러 메서드를 호출해야 한다.
   - 객체가 완전히 생성되기 전 일관성이 무너진 상태이다.
   - 불변으로 만들 수 없다.
3. **빌더 패턴**
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

