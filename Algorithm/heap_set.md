# 힙과 셋 Heap, Set

## Heap
### 우선순위 큐 Priority Queue
- 우선순위를 기준으로 가장 우선순위가 높은 데이터가 가장 먼저 나가는 방식

- 순서가 아닌 우선순위(중요도, 크기 등)를 기준으로 가져올 요소를 결정
  - 가중치가 있는 데이터
  - 작업 스케줄링
  - 네트워크

- 우선순위 큐 => 힙 큐

### 힙의 특징
- 최대값, 최소값을 빠르게 찾아내도록 만들어진 데이터구조(큐와의 차이점)

- 완전 이진트리의 형태로 느슨한 정렬상태를 지속적으로 유지

- 데이터가 지속적으로 정렬되어야 하는 경우, 데이터 삽입/삭제가 빈번한 경우 사용

### 파이썬의 heapq 모듈
- Minheap(최소힙)으로 구현되어 있음

- Maxheap을 구현하고 싶으면 heap에 데이터를 넣고 뺄 때 '-'를 붙이면 됨
  ```python
  heapq.heappush(list, -x)
  print(-heapq.heappop(list))
  ```

- 삽입, 삭제, 수정, 조회 연산의 속도가 리스트보다 빠름

  |  연산 종류  |   힙   |  리스트  |
  |------------|--------|----------|
  |  Get item  |  O(1)  |   O(1)   |
  | Insert item| O(logN)| O(1)/O(N)|
  | Delete item| O(logN)| O(1)/O(N)|
  | Search item|  O(N)  |   O(N)   |

- heapq.heapify(list) : list를 heap으로 변환
- heapq.heappush(heap, item) : item값을 heap으로 push, 튜플일 경우 첫번째인자-두번째인자 순으로 비교
- heapq.heappop(heap) : heap에서 가장 작은 항목 반환 후 삭제
- heapq.heappushpop(heap, item) : item을 push 후 가장 작은 항목 반환 후 pop
- heapq.heapreplace(heap, item) : 가장 작은 항목 반환 & pop 한 후 item을 push


## Set
- set은 수학에서 `집합`을 나타내는 데이터 구조로 Python에서는 기본적으로 제공

- 데이터의 중복이 없어야 할 때, 정수가 아닌 데이터의 삽입/삭제/탐색이 빈번히 필요할 때 사용

### set의 연산
1. .add()
2. .remove()
3. |  : 합집합
4. \-  : 차집합
5. &  : 교집합
6. ^  : 대칭차집합

### set의 시간복잡도

|  연산 종류  | 시간 복잡도 |
|------------|------------|
|    탐색     |    O(1)    |
|    제거     |    O(1)    |
|   합집합    |    O(N)    |
|   교집합    |    O(N)    |
|   차집합    |    O(N)    |
| 대칭 차집합 |    O(N)    |