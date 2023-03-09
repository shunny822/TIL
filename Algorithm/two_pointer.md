# two pointer

## 개념

- 두 개의 포인터 위치를 기록하며 처리하는 알고리즘

- 순차적으로 접근하여 start 지점과 end 지점 사이의 어떠한 값을 도출하는데 사용됨

### BOJ 2003번 수들의 합2

```python
n, m = map(int, input().split())
array = list(map(int, input().split()))
cnt = 0
# end point 0으로 시작
end = 0
interval = 0

# start point 0부터 시작
for start in range(n):
    while interval < m and end < n:
        interval += array[end]
        # end를 하나씩 늘려감
        end += 1
    
    if interval == m:
        cnt += 1
    
    # start 값을 빼면서 앞으로 한 칸씩 전진
    interval -= array[start]

print(cnt)
```