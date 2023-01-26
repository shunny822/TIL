# 스택과 큐 Stack, Queue

## 데이터 구조 & 알고리즘
- 프로그램 = 데이터 구조 + 알고리즘

- 데이터를 다양한 방식으로 저장 + 조회, 삽입, 변경, 삭제(`CRUD`)의 기능 제공


## 스택 Stack
- 쌓는다는 의미로 데이터를 한쪽에서만 넣고 빼는 자료구조

- `후입선출` 방식, LIFO(Last-in First-out)

### Stack 사용
- 괄호 매칭

- 함수 호출(재귀 호출)

- 백트래킹

- DFS, 깊이 우선 탐색



## 큐 Queue
- 한쪽 끝에서 데이터를 넣고 다른 한쪽에서 데이터를 뺄 수 있는 자료구조

- `선입선출` 방식, FIFO(First-in First-out)

- dequeue : 큐의 맨 앞 데어터를 가져오는 행위

- enqueue : 큐의 맨 뒤에 데이터를 삽입하는 행위

### Queue 사용
- 순서와 대기

- 프로세스 관리(데이터 버퍼)

- 클라이언트/서버(Message Queue)

- BFS(너비 우선 탐색)

- 다익스트라-우선순위큐


### 리스트 큐
리스트를 이용한 큐에서는 앞의 데이터가 빠지면서 인덱스가 하나씩 당겨짐, 데이터가 많을 경우 비효율적


### 덱 Deque(Double-Ended Queue)

: 양 방향으로 삽입과 삭제가 자유로운 큐

- 양 방향 삽입 추출이 모두 리스트 큐보다 빠름

- 데이터의 삽입/추출이 많은 경우 시간을 크게 단축시킬 수 있음
```python
from collections import deque

queue = deque(range(1, 5))
queue.append(5)
queue.popleft()
print(*queue)   # 2 3 4 5
```