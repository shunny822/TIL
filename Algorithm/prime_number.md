# 소수 Prime Number

## 개념
> 소수 : 1보다 큰 자연수 중 1과 자기 자신을 제외한 자연수로는 나누어 떨어지지 않는 수

- 2부터 나누어 보며 나머지가 남지 않는지 확인하여 소수 판별

<br>

## 약수의 성질

- 모든 약수는 가운데 약수를 기준으로 곱셈 연산에 대해 대칭을 이룸
  - 16 → 1, 2, 4, 8, 16

- 이러한 성질에 의해 특정 자연수의 모든 약수를 찾을 때 가운데 약수(=제곱근)까지만 확인


### 약수의 성질을 이용한 소수 판별 알고리즘
```python
def is_prime_number(x):
  for i in range(2, int(x**1/2) + 1):
    if x % i == 0:
      return False
  return True

print(is_prime_number(7))
```

- 시간 복잡도 : O(N**1/2)

<br>

## 에라토스테네스의 체

```python
n = 1000
array = [True] * (n+1)

for i in range(2, int(n**1/2) + 1):
  if array[i]:
    j = 2
    while i * j <= n:
      array[i * j] = False
      j += 1

for i in range(2, n+1):
  if array[i]:
    print(i)
```

- 특정한 수의 범위 안에 존재하는 모든 소수를 찾아야 하는 경우 사용

- 시간 복잡도 : O(NloglogN), 선형 시간에 가까울 정도로 매우 빠름

- 각 자연수에 대한 소수 여부를 저장해야 하므로 메모리를 많이 차지함


