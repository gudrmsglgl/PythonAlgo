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