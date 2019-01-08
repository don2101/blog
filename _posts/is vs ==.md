## `is` 와 `==`

`is` : 레퍼런스 체크 (참조 비교)

`==` : 값 체크 (값 비교)

```python
dog = 1
dog == 1
>>> True

dog is 1
>>> True
```

```python
dog = 257

dog == 257
>>> True

dog is 257
>>> False
```

```python
# 서로 다른 메모리 주소 값을 가지고 있기 때문에 아래와 같은 결과가 나오게 된다.
id(dog)
>>> 4338786448

id(257)
>>> 4339142832
```

```python
dog = 1

id(dog)
>>> 4306799776

id(1)
>>> 4306799776
```

- `id()` 는 객체의 주소값과 매칭되는 유니크한 int 값을 반환한다.
- `-5 ~ 256` 의 작은 수들은 파이썬 내부적으로 미리 캐싱되어 있다.
- Python Interpreter에서 [[-5, 256]](https://github.com/python/cpython/blob/4830f581af57dd305c02c1fd72299ecb5b090eca/Objects/longobject.c#L18-L23) 범위의 Integer를 미리 캐싱하고 있기 때문에 발생하는 일이다.

---

```python
def animal():
	cat = 257
	print(cat is 257)
    
animal()
>>> True
```

- **그러나 같은 함수내에서는 동일한 메모리를 바라보게되어 True 를 반환한다.**

---

- string 이 너무 길어도 참조값이 다르다. 

```PYTHON
a = "오늘밤 당번을 뽑기 위해 그룹의 3조 중에서 한명을 랜덤으로 뽑아주세요."
b = "오늘밤 당번을 뽑기 위해 그룹의 3조 중에서 한명을 랜덤으로 뽑아주세요."

a is b
False

a == b
True
```

```python
a = "hphk"

b = "hphk"

a is b
True

a == b
True
```

