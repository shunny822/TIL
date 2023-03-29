# 이분 탐색 Binary Search

## 개요

- 순차 탐색 : 리스트 안에 있는 특정한 데이터를 앞에서부터 하나씩 확인하며 찾는 방법

- 이진 탐색
  - 기본적으로 정렬된 리스트에서 사용 가능

  - 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하여 O(logN)의 시간복잡도를 가짐

  - 시작점, 끝점, 중간점을 이용하여 탐색범위 설정

<br>

## 예제

### 기본 방식
[BOJ 2805번 나무 자르기](https://www.acmicpc.net/problem/2805)

```python
def cut(start, end):
  # 중간점의 높이와 이때의 얻는 나무 길이를 구함
    mid_h = (end + start) // 2
    mid = 0
    for tree, num in trees.items():
        if tree > mid_h:
            mid += (tree - mid_h) * num
    
    # start가 역전되면 범위가 끝난 것이기 때문에 종료
    if start > end:
        return print(mid_h)
    # 현재의 값이 더 작으면 더 낮은 높이로 잘라야 하므로 end에 mid_h-1
    elif mid < m:
        cut(start, mid_h-1)
    # 반대이면 더 높은 높이로 잘라야 하므로 start에 mid_h+1
    else:
        cut(mid_h+1, end)

n, m = map(int, input().split())
trees = {}
# 나무 높이와 그 개수를 딕셔너리로 저장
for h in map(int, input().split()):
    trees[h] = trees.get(h, 0) + 1

# start=0, end=가장 높은 나무로 시작
cut(0, max(trees))
```


### 매개변수를 활용한 이분탐색
[BOJ 2110번 공유기 설치](https://www.acmicpc.net/problem/2110)
```python
n, c = map(int, input().split())
house = [int(input()) for _ in range(n)]
# 정렬된 데이터 필요
house.sort()

# 탐색 범위 지정(공유기 간의 최소 거리)
start = 1
end = house[-1]

# 최적의 결과값을 저장할 변수
result = 0

while start <= end:
    # 공유기를 첫번째 집에 설치하고 시작(최대거리를 위해)
    # 설치된 공유기 개수 1로 시작
    cnt = 1
    # 거리 계산을 위해 마지막으로 공유기 설치한 곳 저장
    flag = house[0]
    mid = (start + end) // 2

    for i in range(1, n):
        # mid보다 거리가 큰 경우에만 설치
        if house[i] - flag >= mid:
            cnt += 1
            flag = house[i]
    
    # 최대 거리를 구해야 하므로 좁은 거리에서부터 늘려가기 => start값 키우기
    # 여기서 cnt가 구하려는 값보다 큰 것은 좁은 거리를 의미
    if cnt >= c:
        # 현재의 값 저장하며 최적의 값으로 갱신
        result = mid
        start = mid + 1
    else:
        end = mid - 1

print(result)
```
- 최댓값을 구하는 경우 작은 값에서부터 큰 값으로 좁혀가고, 최솟값의 경우는 큰 값에서 작은 값으로 좁혀가기