# Java Collection(Set, Map) 정리

- Java의 Collection은 **기본적인 자료구조**를 미리 정의해 놓은 라이브러리
- Collection은 **Interface로 구현**되어 있으며, 이를 상속받아 자료구조 구현
  - Collection을 상속한 Interface: **Set, List, Queue**
- **Map**은 Collection을 상속하지 않고, 따로 구현
- **Array, Collections**는 **Object를 상속**받아 구현



## I. Set

- **중복되지 않는 자료를 저장**하는 자료구조
- 중복되는 자료를 저장해도 **중복을 제거하고 자료 저장**
- generic으로 선언된 Interface로 구현되어 있으며, 주로 이를 상속한 **HashSet**을 통해 사용



### HashSet 주요 메서드

#### (1) add(E item)

- Set에 자료 저장



##### 구현 예시

```java
/* SetEx.java */

public class SetEx <E> {
    public Set<E> set;

    public SetEx() {
        //HashSet에 제네릭을 적용하여 생성
        set = new HashSet<E>();
    }

    public void add(E item) {
        set.add(item);
    }

    public void printAll() {
        //set에 있는 모든 item 출력
        for(E item:  set) {
            System.out.println(item);
        }
    }
}
```

```java
/* main에서 실행 */

public class Main {
    public static void main(String args[]) {
        SetEx setEx = new SetEx();

        setEx.add("a");
        setEx.add("b");
        setEx.add("c");
        setEx.add("d");
        setEx.add("e");
        setEx.add("c");	// 중복은 삽입에서 제외
        setEx.add(1);

        setEx.printAll();
        
    }
}

/* 출력 결과
a
1
b
c
d
e
*/
```





## II. Map

- **Key, Value쌍으로 이루어진 자료**를 저장
- python에서 dictionary와 유사
- **key에 value를 매칭**시켜 저장
- value는 중복될 수 있지만 **key는 중복이 불가능**
  - key를 중복시켜 저장할 경우 가장 **마지막에 요청한 자료를 저장**
-  generic으로 선언된 interface로 구현되어 있으며, 주로 이를 상속한 **HashMap**을 통해 사용



### HashMap 주요 메서드

#### (1) put(K key, V value)

- Map에 key값으로 value를 저장



#### (2)get(K key)

- key에 해당하는 value를 반환



#### (3)entrySet()

- Entry: **key와 value를 쌍으로** 갖고있는 객체
  - **getKey()**, **getValue()** 메서드로 key, value 반환
- Map에 속해있는 **모든 Entry를 Set으로 반환**



#### (4) keySet()

- Map에 속해있는 **모든 key값을 Set으로 반환**



#### (5) values()

- Map에 속해있는 **모든 value를 Collection으로 반환**



##### 구현 예시

```java
/* MapEx.java */

// key, value 모두 generic으로 선언
public class MapEx<K, V> {
    public Map<K, V> hashMap;

    public MapEx() {
        hashMap = new HashMap<K, V>();
    }

    public void add(K key, V value) {
        hashMap.put(key, value);
    }

    public void get(K key) {
        hashMap.get(key);
    }

    public void entries() {
        // Entry를 Set으로 반환
        Set<Map.Entry<K, V>> set = hashMap.entrySet();

        for(Map.Entry<K, V> item: set) {
            System.out.println("Key: " + item.getKey());
            System.out.println("Value: " + item.getValue());
        }
    }

    public void keys() {
        Set<K> set = hashMap.keySet();

        for(K item: set) {
            System.out.println("Key: " + item);
        }
    }

    public void values() {
        Collection<V> col = hashMap.values();

        for(V item: col) {
            System.out.println("Value: " + item);
        }
    }
}
```

```java
/* main에서 실행 */

public class Main {
    public static void main(String args[]) {

        MapEx map = new MapEx();

        map.add(1, "one");
        map.add("a", "aaa");
        map.add(2, "two");
        map.add("b", "bbb");
        map.add(3, "three");
        map.add("c", "ccc");
        map.add("c", "ddd");    //가장 마지막에 선언한 자료를 저장

        map.entries();
        map.keys();
        map.values();

    }
}

/* 출력 결과
Key: 1
Value: one
Key: a
Value: aaa
Key: 2
Value: two
Key: b
Value: bbb
Key: 3
Value: three
Key: c
Value: ddd
Key: 1
Key: a
Key: 2
Key: b
Key: 3
Key: c
Value: one
Value: aaa
Value: two
Value: bbb
Value: three
Value: ddd
*/
```




