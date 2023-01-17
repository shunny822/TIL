# 시간복잡도와 Big-O 표현법

## 알고리즘의 시간 복잡도

> 알고리즘 비교의 기준 : CPU 처리 시간!

### 소요시간 측정
- 성능 측정 시 입력을 통일시킴

- 최악의 입력 n개가 들어온다고 가정하고 측정


### 시간 복잡도(Time Complexity)
- 알고리즘의 수행시간 의미(시간 복잡도가 높다 -> 느린 알고리즘)

- 시간복잡도 계산

  - 사칙연산, 읽고 쓰기, 검증 등 단순 코드 : 한 statement 당 1

  - 조건문 : max((True일 경우 시간), (False일 경우 시간)) -> 최악으로 가정
  ```python
  if (조건):
    code block1
  else:
    code block2
  ```

  - 반복문
  ```python
  for i in range(N):
    code block1      # N 번
  
  for i in range(N):
    for j in range(M):
      code block     # N*M 번
  
  for i in range(N):
    for j in range(N):
      code block1
  for k in range(N):
    code block2      # N**2 + N 번

  for i in range(N):
    for j in range(i, N):
      code block1    # N + N-1 + N-2 ... + 1
  ```

  <br>
  <br>

## Big-O 표기법

### Big-O 표기법이란?
1\) 6n + 4

2\) 3n + 2

3\) 3n² + 6n + 1

> 1번과 2번은 2배 차이가 나지만 n이 무한대로 커진다면 차이가 무의미할 것이다.
>
> 입력이 무한대로 커진다고 가정하여 최고차항만 남기고 계수와 상수 제거
> 
> 정확한 수식을 구하는 것은 불필요하며 정확한 수치보다 증가울에 초점을 맞춘다.

1, 2) O(n)

3\) O(n²)

### 다양한 시간 복잡도
![시간복잡도](시간복잡도.png)

<1초가 걸리는 입력의 크기>
|   Big-O   | 크기  |  Big-O | 크기 |
|-----------|-------|--------|------|
|    O(N)   |  1억  |  O(N³) | 500  |
| O(N*logN) | 500만 | O(2^N) |  20  |
|   O(N²)   |  1만  |  O(N!) |  10  |

- O(1) : 단순 계산
- O(logN) : 이진탐색(Binary Search), 분할정복(Divide & Comquer)
  ex) index가 없는 사전에서 단어를 찾을 때, 절반 나누어 아닌쪽 잘라내기 반복
- O(N) : 리스트 순회, for문
- O(NlogN) : 높은 성능의 정렬(Merge/Quick/Heap sort)
- O(N^2) : 2중 리스트순회, for문
- O(N^3) : 3중 리스트순회, for문
- O(2^N) : 크기가 N인 집합의 부분집합
- O(N!) : 크기가 N인 순열