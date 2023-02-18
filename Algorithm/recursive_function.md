# 재귀함수 Recursive function

## 재귀함수의 개념

- `자기 자신`을 다시 호출하는 함수

- 재귀함수 사용 시 종료 조건을 명시해야 함(그렇지 않으면 무한루프로 오류 발생)

- 예시
  ```python
  def recursive_function(i):
    if i == 100:
      return
    print(i, '번째 재귀함수에서', i+1, '번째 재귀함수 호출')
    recursive_function(i+1)
    print(i, '번째 재귀함수 종료')

  recursive_function(1)
  ```

<br>

## 재귀함수의 사용

### 팩토리얼
n! = 1 x 2 x 3 x ... x (n-1) x n
```python
def factorial(n):
  if n <= 1:
    return 1
  return n * factorial(n-1)
```


### 유클리드 호제법
- 두 개의 자연수에 대한 최대공약수를 구하는 알고리즘
- 두 자연수 A, B와 A를 B로 나눈 나머지 R이 있을 때, A, B의 최대공약수는 B, R의 최대공약수와 같음

  |   |  A  |  B  |
  |---|-----|-----|
  | 1 | 192 | 162 |
  | 2 | 162 | 30  |
  | 3 | 30  | 12  |
  | 4 | 12  |  6  |

  → 12는 6의 배수, 최대공약수 = 6

```python
def gcd(a, b):
  if a % b == 0:
    return b
  else:
    return gcd(b, a%b)

gcd(162, 192) # 6
# a, b의 크기 상관없음. b가 크더라도 gcd 한번 돌면 큰 수가 a, 작은 수가 b가 됨
```


### Python 재귀 깊이

- 파이썬은 최대 재귀 깊이가 1000으로 설정되어 있음

- 그 이상으로 재귀함수를 사용하는 경우 다음과 같은 코드로 재귀깊이를 변경
  ```python
  import sys
  sys.setrecursionlimit(10**6)
  # 10**6으로 set
  ```