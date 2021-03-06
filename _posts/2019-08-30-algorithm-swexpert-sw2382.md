---
layout: post
title: "SW expert 2382. 미생물 격리 -python"
subtitle: "1949.등산로 조성"
date: 2019-08-30 21:00:00 +0900
categories: algorithm
tags: swexpert
comments: true
---

## SWEA 2382. 미생물 격리 -python

<br>

`문제풀이`

- 시뮬레이션 문제

- 주어진 조건들을 잘 고려해서 처리해 주면 된다

  1. 1시간마다 이동방향에 있는 다음 셀로 이동
  2. 이동 후 약품이 칠해진 곳에 도착하면 미생물의 절반이 소멸
  3. 이동방향 바꿔주기
  4. 두 개 이상의 미생물 군집이 한 셀의 모이는 경우 미생물의 수를 더해주기
  5. 이동방향은 군집들 중 가장 많은 군집의 이동방향을 따른다

- 나머지 조건들은 그냥 처리해주면 되지만 `두 개 이상의 군집`에서 3개 이상의 군집이 되었을 때를 처리해줘야한다

  - 예를들어 120 80 150의 미생물 집단이 있을 때 120 80중 큰것은 120이고 이를 더해주면 200이 된다
  - 그 후 200과 150을 비교했을 때 원래는 120 80 150중 150이 가장 크기 때문에 150을 선택해 주어야 하지만 이미 비교 후 더해버린 200과 150을 비교하면 안되므로 이 부분을 처리해 줘야한다.
  - 밑의 코드에서는 maxV에 같은것 중 큰 미생물 개수를,  mavT에는 방향을 저장하고 첫번째로 나오는 미생물에 덮어씌워 주었다.

  > 이 문제를 풀고 다른 친구와 코드리뷰를 했는데 정렬을 해버리면 번잡한 처리 과정이 필요가 없었다는걸 깨닳았다. 문제는 정렬을 어떻게 해주냐인데 `lamda`를 통해 `미생물 수`를 기준으로 정렬하면 된다고 했다.
  >
  > `lamda`에대해서 조금 알아봐야겠다고 생각했다.

<br>

`code`

```python
T = int(input())

def move(arr, idx):
    if(arr[idx][3] == 1):
        arr[idx][0] -= 1
    elif(arr[idx][3] == 2):
        arr[idx][0] += 1
    elif(arr[idx][3] == 3):
        arr[idx][1] -= 1
    elif(arr[idx][3] == 4):
        arr[idx][1] += 1

def color(arr, idx):
    if(arr[idx][0] == 0 or arr[idx][1] == 0 or arr[idx][0] == n-1 or arr[idx][1] == n-1):
        arr[idx][2] = int(arr[idx][2]/2)
        if(arr[idx][3] == 1):
            arr[idx][3] = 2
        elif(arr[idx][3] == 2):
            arr[idx][3] = 1
        elif(arr[idx][3] == 3):
            arr[idx][3] = 4
        elif(arr[idx][3] == 4):
            arr[idx][3] = 3

def check(arr):
    for i in range(k):
        maxV = arr[i][2]
        maxT = arr[i][3]
        ssum = arr[i][2]
        for j in range(i+1,k):
            if(arr[i][0] == arr[j][0] and arr[i][1] == arr[j][1]):
                if(maxV < arr[j][2]):
                    ssum += arr[j][2] 
                    maxV = arr[j][2]
                    maxT = arr[j][3]
                    arr[j][2] = 0
                    arr[j][3] = 0
                else:
                    ssum += arr[j][2]
                    arr[j][2] = 0
                    arr[j][3] = 0
        arr[i][2] = ssum
        arr[i][3] = maxT

for tc in range(T):
    n, m, k = map(int, input().split())
    arr = [[0]*4 for i in range(k)]
    for i in range(k):
        arr[i] = list(map(int, input().split()))
    while(m > 0):
        for i in range(k):
            move(arr, i)
            color(arr, i)
        check(arr)
        m -= 1

    res = 0
    for i in range(len(arr)):
        res += arr[i][2]
    print("#{} {}".format(tc+1, res))
```

