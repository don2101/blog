## Java Reflection



### I. 기본 개념

- 객체를 통해 클래스의 정보를 분석해내고 사용할 수 있게 하는 Java Api
- 구체적인 클래스 타입을 알지 못해도 컴파일된 바이트 코드를 통해 클래스의 정보를 알아내서 클래스를 사용
- 객체의 타입을 모르는 상태에서 객체의  메서드를 호출할 수 있다.
- 자바에서 동적 바인딩 구현
- 코드를 작성하는 시점에 명확한 클래스가 존재하지 않아도 된다



### II. 사용 이유

1. Composition과 함게 다형성을 구현하기 위해 사용
2. 프레임워크 유연성 제공
3. 코드 작성 시점에 클래스의 타입을 모르는 경우에 사용



### III. 작동 방식

- Java 클래스 파일은 바이트 코드로 컴파일 되어 Static 영역에 위치
- 클래스 이름을 통해 Static 영역에서 클래스에 대한 정보를 추출







### V. 참고

[자바의 리플렉션](https://brunch.co.kr/@kd4/8)

[리플렉션 이란?](http://asfirstalways.tistory.com/221)