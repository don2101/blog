# 초기화 블럭

- 클래스 초기화 블럭 : 클래스 변수의 초기화에 사용된다. 클래스가 처음 로딩될 때 한번만 수행.
- 인스턴스 초기화 블럭 : 인스턴스 변수의 초기화에 사용된다. 인스턴스가 생성될 때 마다 수행.(생성자 보다 먼저 수행된다)



### 예시

```java
class Block {
    static {
        /* 클래스 초기화 블럭 */
    }
    
    {
        /* 인스턴스 초기화 블럭 */
    }
}
```

- 보통 인스턴스 변수 초기화는 생성자를 사용하기  때문에 인스턴스 초기화 블럭은 잘 사용되지 않는다. 
- 하지만, 모든 생성자에서 공통적으로 수행해야 하는 코드를 인스턴스 초기화 블럭에 넣어 코드의 중복을 줄일 수 있다.