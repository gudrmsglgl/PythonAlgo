## 리스트 컴프리헨션

```python
>>> list(map(lambda x: x + 10, [1,2,3])) 
[11,12,13]
```


- 다음은 홀수인 경우 2를 곱해 출력하라는 리스트 컴프리헨션
```python
>>> [n * 2 for n in range(1, 10 + 1) if n % 2 == 1]
[2, 6, 10, 14, 18]
```

- 리스트 컴프리헨션을 사용하지 않는다면
```python
>>> a = []
>>> for n in range(1, 10 + 1):
...     if n % 2 == 1:
...         a.append(n*2)
>>> a
[2, 6, 10, 14, 18]
```

- 딕셔너리 
```python
>>> a = {}
>>> for key, value in original.items():
...     a[key] = value
```

- 딕셔너리 컴프리헨션
```python
>>> a = {key: value for key, value in original.items()}
```

- 컴프리헨션은 가독성이 좋은 편이지만 가독성이 떨어뜨릴 수 있음
- 대체로 표현식은 2개를 넘지 않아야 한다.

## 제너레이터
- 루프의 반복(Iteration) 동작을 제어할 수 있는 루틴 형태를 말한다.
- 임의의 조건으로 숫자 1억 개를 만들어내 계산하는 프로그램 작성 가정
    - 제너레이터가 없다면 메모리 어딘가에 만들어낸 숫자 1억 개를 보관하고 있어야 한다.
    - 그러나 제너레이터를 이용하면, 단순히 제너레이터만 생성해두고 필요할 때 언제든 숫자를 만들어낼 수 있다.
    - 만약 1억 개 중 100개 정도만 쓰인다면 차이는 더욱 클 것.
- yield 구문을 사용하면 제너레이터를 리턴할 수 있다.
- 기존의 함수는 return 구문을 맞닥뜨리면 값을 리턴하고 모든 함수의 동작을 종료한다.
- 그러나 yield는 제너레이터가 여기까지 실행 중이던 값을 내보낸다는(단어의 사전적 의미처럼 '양보하다')의미로
- 중간값을 리턴한 다음 함수는 종료되지 않고 계속해서 맨 끝에 도달할 때까지 실행된다.
- 다음 코드의 경우처럼 while True 구문은 종료 조건이 없으므로 계속해서 값을 내보낼 수 있다.
```python
>>> def get_natural_number():
        n = 0
        while True:
            n += 1
            yield n
```

- 이 경우 함수의 리턴 값은 다음과 같이 제너레이터가 된다.
```python
>>> get_natural_number()
<generator object get_natural_number at 0x10d3139d0>
```

- 만약 다음 값을 생성하려면 next()로 추출하면 된다.
- 예를 들어 100개의 값을 생성하고 싶다면 다음과 같이 100번 동안 next()를 수행하면 된다.
```python
>>> g = get_natural_number()
>>> for _ in range(0, 100):
        print(next(g))
...
1
2
3
...
98
99
100
```

- 아울러 제너레이터는 다음과 같이 여러 타입의 값을 하나의 함수에서 생성하는 것도 가.
```python
>>> def generator():
...     yield 1
...     yield 'string'
...     yield True
>>> g = generator()
>>> g
<generator object generator at 0x10a47c678>
>>> next(g)
1
>>> next(g)
'string'
>>> next(g)
True
```

## range
- 제너레이터의 방식을 활용하는 대표적인 함수 -> range()
- 주로 for 문에서 쓰이는 range() 함수의 쓰임은 다음과 같다.
```python
>>> list(range(5))
[0, 1, 2, 3, 4]
>>> range(5)
range(0, 5)
>>>type(range(5))
<class 'range'>
>>> for i in range(5):
...     print(i, end=' ')
...
0 1 2 3 4 
```

- 이 코드에서 range()는 range 클래스를 리턴하며, <br>
  for 문에서 사용할 경우 내부적으로는 제너레이터의 next()를 호출하듯 매번 다음 숫자를 생성해내게 된다.
  
- 버전 3 이후, range() 함수가 제너레이터 역할을 하는 range 클래스를 리턴하는 형태로 변경 xrange() 함수는 사라짐.
- 생성할 숫자가 100만 개쯤 될 때
  - [x] 제너레이터를 리턴하듯 range 클래스만 리턴하면 그렇지 않다.
  - [x] 생성 조건만 정해두고 나중에 필요할 때 생성해서 꺼내 쓸 수 있다.
  
- 숫자 100만 개를 생성하는 2가지 방법
  - [x] ```>>> a = [n for n in range(1000000)] // list ```
  - [x] ```>>> b = range(1000000)  // range ```
  
```python
>>> len(a)
1000000
>>> len(b)
1000000
>>> len(a) == len(b)
True
```
- 그러나 a에는 이미 생성된 값이 담겨 있고, b는 생성해야 한다는 조건만 존재
```python
a = [0, 1, 2, 3, ... , ] // a는 리스트로서 값이 존재함
b = range(0, 1000000) // b는 클래스임

// 메모리 점유율 
sys.getsizeof(a)
8448728
sys.getsizeof(b)
48
```
- b는 생성 조건만 보관하고 있기 때문에 1억 개라도 b 변수의 메모리 점유율은 동일.
- 게다가 미리 생성하지 않은 값은 인덱스에 접근이 안될 것라 생각할 수 있으나, 인덱스로 접근 시에는 바로 생성하도록 구현있음

```python
b[999] // b를 리스트처럼 인덱스로 바로 접근해서 쓸 수 있음 
999
```
## enumerate
- enumerate()는 '열거하다' 뜻의 함수로, 여러 가지 자료형 (list, set, tuple 등) 인덱스를 포함한 enumerate 객체로 리턴한다.

```python
>>> a = [1,2,3,2,45,2,5]
>>> a
[1,2,3,2,45,2,5]
>>> enumerate(a)
<enumerate object at 0x7fe4c0201e00>
>>>list(enumerate(a))
[(0, 1), (1, 2), (2, 3), (3, 2), (4, 45), (5, 2), (6, 5)]
```
- 이처럼 list()로 결과를 추출할 수 있는데, 인덱스를 자동으로 부여해주기 때문에 매우 편리하게 활용.
- a = ['a1', 'b2', 'c3'] -> 인덱스와 함께 출력할려면?

```python
for i in range(len(a)):
    print(i, a[i])
-------------------------
0 a1
1 b2
2 c3
```
- 위와 같은 방법은 값을 가져오기 위해 불필요한 a[i] 조회 작업과 전체 길이를 조회하여 루프를 처리

```python
i = 0
for v in a:
    print(i, v)
    i += 1
-------------------------
0 a1
1 b2
2 c3
```
- 위와 같은 방법은 값은 깔끔하게 처리했으나 인덱스를 위해 변수를 별도로 관리하는 형태 깔끔하지 않음.
- 가장 깔끔한 방식은 아래와 같이 enumerate()를 활용
```python
for i, v in enumerate(a):
    print(i, v)
-------------------------
0 a1
1 b2
2 c3

```

## 나눗셈 연산자
- 파이썬 2 이하에서 기본 나눗셈 연산자 / 는 타입을 유지하는 특성 때문에 실수하기 쉬웠음.
- 5/3 기대하는 결과 -> 1.666 but!! 파이썬 2이하 버전에서는 정수형을 유지해 다음과 같이 1 리턴.
- // 연산자가 파이썬 2 이하 버전의 정수형을 나눗셈할 때 동일한 정수형을 결과로 리턴하면서 내림 연산자의 역할
- 즉 몫을 구하는 연산자.

```python
5/3
1.6666666666666667
--------------------------------
type(5/3)
<class 'float'>
--------------------------------
5 //3
1
--------------------------------
type(5//3)
<class 'int'>
--------------------------------
int(5/3)
1
--------------------------------
type(int(5/3))
<class 'int'>
```

- 나머지 연산
```python
5 % 3
2
```

- 몫과 나머지 동시에 구하려면 divmod() 함수 사용. (자주 사용 예정)
```python
divmod(5,3)
(1, 2)
```

## print
- 디버깅을 할 때 가장 자주 쓰는 명령은 바로 print()
- 정답 제출시에는 print() 조차 보여주지 않는 경우가 있으니 유의.
- 가장 쉽게 값을 출력하는 방법은 콤마(,)로 구분하는 것.

```python
print('A1', 'B2')
A1 B2

print('A1', 'B2', sep="||")
A1||B2
```

- print() 함수가 항상 줄바꿈을 하기 때문에 긴 루프의 값을 반복적으로 출력하면 디버깅 하기가 어려움
- 이 경우 end 파라미터를 공백으로 처리하여 줄바꿈을 하지 않도록 제한할 수 있다.
```python
>>> print('aa', end='')
... print('bb')
aabb
```

- 리스트를 처리할 때
```python
a = ['A', 'B']
print(' '.join(a))
A B
print('^^'.join(a))
A^^B
```

- format 을 활용한 print
```python
print('{0}: {1}'.format(idx + 1, fruit))
2: Apple
--------------------------------------------
print('{}: {}'.format(idx + 1, fruit))
2: Apple
--------------------------------------------

// f-string 3.6+ 에서만 지원.
print(f'{idx +1}: {fruit}')
2: Apple
```

## pass
- 일단 코드의 전체 골격을 잡아 놓고 내부에서 처리할 내용은 차근차근 생각하며 만들겠다는 의도로 다음과 같이 코딩하는 경우가 있다.
```python
class MyClass(object):
    def method_a(self):
    
    def method_b(self):
        print("Method B")

c = MyClass()
```
- 이 클래스는 실행이 되지 않는다. 다음과 같이 인텐트 오류가 발생.
```python
 def method_b(self):
    ^
IndentationError: expected an indented block
```

- 이 문제는 method_a() 가 아무런 처리를 하지 않았기 때문에 엉뚱하게 method_b()에서 오류.
- 이런 오류 생각보다 처리하기 번거롭다.
- pass는 이런 오류를 막는 역할을 한다.
- 다음과 같이 pass를 method_a()에 삽입해 간단히 처리할 수 있다.

```python
class MyClass(object):
    def method_a(self):
      # 여기에 pass 추가
        pass
    
    def method_b(self):
        print("Method B")

c = MyClass()

<__main__.MyClass object at 0x7fd9d814a9a0>
```

- 파이썬에서 **pass는 널 연산 Null Operation** 으로 아무것도 하지 않는 기능.
- 이처럼 아무 역할을 하지 않는 pass를 지정하면, 앞서 발생한 인텐트 오류 같은 불필요한 오류를 방지할 수 있다.
- 이렇게 pass는 먼저 목업 mockup 인터페이스부터 구현한 다음에 추후 구현을 진행할 수 있게 한다.
- 코딩 테스트 시 무척 유용
