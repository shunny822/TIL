# 이분 탐색 Binary Search

## 개요

- 순차 탐색 : 리스트 안에 있는 특정한 데이터를 앞에서부터 하나씩 확인하며 찾는 방법

- 이진 탐색
  - 기본적으로 정렬된 리스트에서 사용 가능

  - 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하여 O(logN)의 시간복잡도를 가짐

  - 시작점, 끝점, 중간점을 이용하여 탐색범위 설정

<br>

## 예제

[BOJ 2805번 나무 자르기](https://www.acmicpc.net/problem/2805)

```python
import sys
input = sys.stdin.readline

def cut(start, end):
  # 중간점의 높이와 이때의 얻는 나무 길이를 구함
    mid_h = (end + start) // 2
    mid = 0
    for tree, num in trees.items():
        if tree > mid_h:
            mid += (tree - mid_h) * num
    
    # 얻은 나무의 합이 구하려는 값과 같거나
    # 범위가 계속 줄어 중간점이 시작점과 같아진 경우 프린트 후 종료
    if mid == m or start == mid_h:
        return print(mid_h)
    elif mid < m:
        cut(start, mid_h)
    else:
        cut(mid_h, end)

n, m = map(int, input().split())
trees = {}
# 나무 높이와 그 개수를 딕셔너리로 저장
for h in map(int, input().split()):
    trees[h] = trees.get(h, 0) + 1

# start=0, end=가장 높은 나무로 시작
cut(0, max(trees))
```