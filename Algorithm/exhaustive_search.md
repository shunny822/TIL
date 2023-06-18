# 완전탐색 Exhaustive Search

## 1. Brute-force

> 모든 경우의 수를 탐색하여 문제를 해결하는 방식

- Brute-force는 무식하게 밀어붙인다는 뜻

- 단순 조건문과 반복문을 이용하여 풀 수 있음

### 블랙잭 예제
n개의 카드 중 세개를 골라 m 이하이며 m과 가장 가까운 합 만들기
```python
n = 5
m = 21
card = [5, 6, 7, 8, 9]
blackjack = 0

# 중복되는 경우 방지하고자 인덱스 조작
for i in range(0,n-2):
  for j in range(i+1, n-1):
    for k in range(j+1, n):
      three = card[i] + card[j] + card[k]
      if three <= m and three > blackjack:
        blackjack = three

print(blackjack)
```

<br>

## 2. Delta Search

![델타탐색](img/%EB%8D%B8%ED%83%80%ED%83%90%EC%83%89.jpg)

- (0, 0)에서부터 이차원 리스트의 모든 원소를 순회하며 각 지점의 상하좌우 조회/이동

- 상하좌우 탐색을 위해 인덱스를 조작

- 행과 열의 변량인 -1, +1을 델타delta값이라고 함

### 델타값을 이용한 상하좌우 이동

```python
# 행은 x 또는 r로, 열은 y 또는 c로 표현
dx(또는 r) = [-1, 1, 0, 0]
dy(또는 c) = [0, 0, -1, 1]

for i in range(4):
  nx = x + dx[i]
  ny = y + dy[i]

  # 3*3 행렬의 인덱스범위 고려
  if 0 <= nx <= 3 and 0 <= ny <= 3:
    x = nx
    y = ny
```

### 델타값의 튜플 사용
```python
delta = [(-1, 0), (1, 0), (0, -1), (0, 1)]

for dx, dy in dalta:
  nx = x + dx
  ny = y + dy
```