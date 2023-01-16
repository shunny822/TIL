# 데이터 구조와 알고리즘

> 프로그램 = 데이터 구조 + 알고리즘

- 데이터 구조 : 데이터를 다양한 방식으로 저장

- 구조를 안다는 것은 어떻게 저장하고 어떻게 활용할 것인가를 아는 것

- 데이터를 필요에 따라 저장하여 문제를 더 효율적으로 해결

- 데이터 구조 : Array, Linked List(연결리스트), Hash), Stack, Queue, Priority Queue(우선순위 큐), Heap, Tree, Graph

- 알고리즘
  - 기본 : 완전탐색, 재귀, 시뮬레이션, 그리디
  - 심화 : DFS, BFS, 백트래킹, 이진탐색, DP, 다익스트라, 크루스칼, 프림

<br>

## 입력과 출력

### Input 활용
- 기본
  ```python
  # 문자열
  a = input()

  # 숫자 1개
  a = int(input())

  # 공백으로 나누어진 숫자 여러 개
  a, b = map(int, input().split())
  ```

- map() 함수
  ```python
  a, b, c = map(int, ['1', '2', '3'])
  print(a, b, c) # 1 2 3

  # 리스트뿐만 아니라 문자열에도 적용 가능!!!
  a, b, c = map(int, '123')
  print(a, b, c) # 1 2 3

  a, b = map(int, '1 2'.split())
  print(a+b) # 3
  ```

- sys : File Object, 사용자의 입력을 buffer에 두었다가 필요할 때 꺼냄
  ```python
  import sys
  a = sys.stdin.readline() # readline으로 한줄씩 읽어오기
  ```


### Output 활용
- print() : end의 기본값은 개행('\n'), sep의 기본값은 space, 필요에 따라 변경하여 사