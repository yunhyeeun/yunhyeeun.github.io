---
title: Stack & Queue 문제 풀기
categories: 
    - Develop
tags:
    - Data Structure
    - python
    - programmers
---

## 탑

> 수평 직선에 탑 N대를 세웠습니다. 모든 탑의 꼭대기에는 신호를 송/수신하는 장치를 설치했습니다. 발사한 신호는 신호를 보낸 탑보다 높은 탑에서만 수신합니다. 또한, 한 번 수신된 신호는 다른 탑으로 송신되지 않습니다.
맨 왼쪽부터 순서대로 탑의 높이를 담은 배열 heights가 매개변수로 주어질 때 각 탑이 왼쪽으로 쏜 신호를 어느 탑에서 받았는지 기록한 배열을 return 하도록 solution 함수를 작성해주세요.
신호를 수신하는 탑이 없으면 0으로 표시합니다.
예시) heights = [6,9,5,7,4] output = [0,0,2,2,4]
             = [3,9,9,3,5,7,2] output = [0,0,0,3,3,3,6]
             = [1,5,3,6,7,6,5] output = [0,0,2,0,0,5,6]

Stack이 뭔지 몰라도 쉽게 풀이를 생각할 수 있다.   
문제가 말하는 순서대로 코드를 짜면 된다.
heights의 마지막 element부터 iteration하며 자기보다 더 큰 value를 갖는 element의 index를 찾으면 된다.
Stack이 카데고리에 있는 이유는 Stack이 LIFO이기 때문이다.
height의 첫 element부터 stack에 차곡차곡 쌓은 다음 pop한 element 별로 value를 비교하면 되다.

```
def solution(heights):
    answer = []
    for i in range(len(heights)-1, -1, -1):
        send = heights[i]
        for j in range(i-1, -1, -1):
            receive = heights[j]
            if receive > send:
                answer.append(j+1)
                break
        if len(answer) != len(heights) - i:
            answer.append(0)
    answer.reverse()
    return answer

```

## 다리를 지나는 트럭

> 트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.   
solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

```
def solution(bridge_length, weight, truck_weights):
    numTruck = len(truck_weights)
    time = [0] * numTruck
    truckInBridge = []
    passedTruck = []
    while len(passedTruck) < numTruck:
        if (len(truck_weights) > 0 and sum(truckInBridge) + truck_weights[0] <= weight):
            truckInBridge.append(truck_weights.pop(0))
        time[:len(passedTruck) + len(truckInBridge)] = [x+1 for x in time[:len(passedTruck) + len(truckInBridge)]]
        if time[len(passedTruck)] == bridge_length:
            passedTruck.append(truckInBridge.pop(0))
    print (truck_weights)
    print (time)
    answer = time[0] + 1
    return answer
```

## 기능개발

> 먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.   
progresses	speeds	return   
[93,30,55]	[1,30,5]	[2,1]   

```
def solution(progresses, speeds):
    answer = []
    numProgress = len(progresses)
    while (sum(answer) != numProgress):
        while progresses[0] < 100:
            for i in range(len(progresses)):
                progresses[i] += speeds[i]
        finished = []
        for i in range(len(progresses)):
            if progresses[0] >= 100:
                finished.append(progresses.pop(0))
                speeds.pop(0)
        answer.append(len(finished))
    return answer
```