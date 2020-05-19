---
title: 프로그래머스 코딩테스트 level 2
categories: 
    - Develop
tags:
    - codingtest
    - python
    - programmers
---
***
프로그래머스 Level2를 했다. Level1과 난이도 차이는 크지 않았다.   

## 행렬 곱 구하기

>입력받은 arr1, arr2의 곱을 구하세요.   
주어지는 입력값은 모두 행렬곱이 가능한 형태입니다.   
arr1 = [[1,2],[3,4],[2,3]], arr2 = [[3,1],[2,4]], result = [[7,9],[17,19],[12,14]]

행렬 곱하는 방법 그대로 구현하면 된다.   
행렬 A, B가 있고 A = p×q, B = q×r이면 A×B = p×r 사이즈의 행렬이 된다.   

```
def solution(arr1, arr2):
    answer = []

    for i in range(len(arr1)):
        tmp = []
        for j in range(len(arr2[0])):
            e = 0
            for k in range(len(arr1[i])):
                e += arr1[i][k] * arr2[k][j]
            tmp.append(e)
        answer.append(tmp)
    return answer
```

## 접두사 구하기

>입력받은 phone_book에서 접두사가 있는지 구한다.      
phone_book = ["119", "1235", "11923259"], result = False   
phone_book = ["542", "35234", "625341"], result = True

for loop을 돌며 접두사인지 확인하면 된다.    
List element가 string이어서 startswith 함수를 쓰면 간편하다.   
```
def solution(phone_book):
    for num in phone_book:
        numlist = [x for x in phone_book if num !=x and x.startswith(num)]
        if len(numlist) > 0:
            return False
    return True
```