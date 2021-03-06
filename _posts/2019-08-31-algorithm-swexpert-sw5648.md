---
layout: post
title: "SW expert 5648. 원자 소멸 시뮬레이션 -python"
subtitle: "5648.원자소멸"
date: 2019-08-31 17:00:00 +0900
categories: algorithm
tags: swexpert
comments: true
---

## SWEA 5648. 원자 소멸 시뮬레이션 -python

<br>

`문제풀이`

- 시뮬레이션 문제

- 주어진 조건들을 해결한다

  - 이동중에 원자들이 만나는 경우가 있기 때문에 1만큼 이동시키는게 아니라 0.5만큼 이동시킴
  - 0.5만큼 이동하기 때문에 4000정도로 돌려줌
  - `x,y 좌표중 -1000 ~ 1000의 범위를 넘어가면 제거를 해줘야함`

  > 이 부분을 해결해주지 않으면 시간초과에 걸림...

- 딕셔너리를 이용해서 문제를 해결

  - 딕셔너리의 원본을 유지하면서 리스트 값을 추가해주기 위해 `try` / `except` 를 이용해서 해결

<br>

`code`

```python
T = int(input())

# map으로 0.5만큰 움직이기
def move(i):
    dy, dx = d[i[2]]
    return [i[0]+dx, i[1]+dy, i[2], i[3]]

# dic에 i를 병합해주기 (key, value값을 유지하면서)
## i가 없어서 병합에 실패하면 except문 실행
### 즉, x,y가 같은, 원자들이 만나서 충돌하게되면 에너지값을 더해주기위해 value에 i를 추가해줌
def dicts(i):
    x, y = i[0], i[1]
    try:
        dic[(x,y)].append(i)
    except:
        dic[(x,y)] = [i]        

for tc in range(T):
    n = int(input())
    arr = []
    for i in range(n):
        arr.append(list(map(int, input().split())))
    d = [(0.5,0), (-0.5,0), (0,-0.5), (0,0.5)]
    energy = 0
    for _ in range(4000):
        if(len(arr) < 2):
            break
        dic = {}
        arr = list(map(move, arr))
        list(map(dicts, arr))
        arr = []
        for i in dic:
          	# 원자들이 충돌해서 value값이 2개 이상일 때
            if(len(dic[i]) > 1):
                item = dic[i]
                for en in item:
                    energy += en[3]
            # value값이 1개일 때
            ## -1000 ~ 1000 범위 내에 있으면 arr에 다시 추가해줌
            else:
                item = dic[i]
                x = item[0][0]
                y = item[0][1]
                if(x < -1000 or x > 1000 or y < -1000 or y > 1000):
                    pass
                else:
                    arr.append(item[0])    
    print("#{} {}".format(tc+1, energy))

```

