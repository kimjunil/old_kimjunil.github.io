---
layout: post
title: "[프로그래머스] LV2 더 맵게"
date: 2021-12-29 13:54
category: algorithm
author: Junil
tags: [algorithm, programmers]
summary: "[프로그래머스] LV2 더 맵게 문제풀이"
---

# Try 1
```python
def solution(scoville, K):
    
    scoville.sort()
    
    count = 0
    
    while min(scoville) < K:
        if len(scoville) <= 1:
            return -1
        
        first_low_scov = scoville.pop(0)
        second_low_scov = scoville.pop(0)
        mix_scov = first_low_scov + (second_low_scov*2)
        
        
        scoville.append(mix_scov)
        scoville.sort()
        
        count += 1
    
    return count
```

# Try 2
heapq를 사용하지 않고 풀어보려고 했으나 효율성테스트에서 계속 실패해했다.
결국 heapq를 사용했다.
```python
import heapq

def solution(scoville, K):
    
    heapq.heapify(scoville)
    
    count = 0
    
    while scoville[0] < K:
        if len(scoville) <= 1:
            return -1
        
        first_low_scov = heapq.heappop(scoville)
        second_low_scov = heapq.heappop(scoville)
        mix_scov = first_low_scov + (second_low_scov*2)
        
        heapq.heappush(scoville, mix_scov)
        count += 1
    
    return count
```


# 알게된 점
python에서는 heapq 라는 내장 모듈을 통해 리스트를 heap처럼 사용할 수 있다.
heapq는 이진트리 기반의 min heap 자료구조를 제공한다. 가장 작은 값이 항상 이진트리의 root가 되고 모든 원소는 항상 자식 원소보다 작거나 같다.
```python
import heapq

# heapq의 주요 메소드
data = [3,6,1,4,8,2.9]

heapq.heapify(scoville)
# data ->[1,2,3,4,6,8,9]

a = heapq.heappop(data) 
# a -> 1
# data -> [2,3,4,6,8,9] 

heapq.heappush(data, 5)
# data -> [2,3,4,5,6,8,9] 
```