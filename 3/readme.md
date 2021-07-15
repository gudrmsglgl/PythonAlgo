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