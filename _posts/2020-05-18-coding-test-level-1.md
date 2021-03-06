---
title: 프로그래머스 코딩테스트 level 1
categories: 
    - Develop
tags:
    - codingtest
    - python
    - programmers
---
***
프로그래머스 코딩테스트를 해보았다. Level 1이라고 만만하게 봐서는 안된다.

## 소수 찾기
> 1부터 입력받은 숫자 n 사이에 존재하는 소수의 개수를 반환하는 함수, solution을 만들어보세요.   
소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다.   
(1은 소수가 아닙니다.)   
예시) 1부터 10 사이의 소수는 [2, 3, 5, 7] 4개가 존재하므로 4를 반환

해결방법은 간단하다. 입력받은 n만큼 for loop을 돌면서 소수인지 체크하는 함수를 만들고 이 함수를 다시 n만큼 for loop을 돌면 된다.

```
def is_prime(n):
    if n == 1:
        return False
    for i in range(1, n):
        if n%i == 0 and i != 1:
            return False
    return True

def solution(n):
    cnt = 0
    for i in range(1,n+1):
        if is_prime(i):
            cnt += 1
    return cnt
```
이 방법을 사용하면 소수를 찾을 수 있지만 이중 for문을 돌아서 시간이 오래 걸리는 것이 단점이다.   
후반 테스트 케이스와 효율성 테스트에서 fail이 뜨는 것도 그 때문이다.   
소수가 아님을 확인하면 for문을 다 돌기 전에 break하거나 입력받은 n의 루트값까지만 for문을 도는 정도로는 효율성 테스트를 통과할 수 없다.   
새로운 아이디어가 필요하다.

## 에라토스테네스의 체

고대 그리스 수학자 에라토스테네스의 체가 발견한 소수 찾는 방법이다.   
1. 1부터 n까지 존재하는 모든 자연수를 적은 다음 차례대로 소수인지 아닌지를 체크한다.   
2. 2가 소수임을 확인한 후에는 2의 배수를 모두 지운다.   
3. 남아있는 수 가운데 3이 가장 작은 소수임을 확인한 후에는 3의 배수를 모두 지운다.   
4. 위 과정을 반복하면 소수만 남는다.   

```
def solution(n):
    prime = [i for i in range(3, n+1, 2)]
    for i in range(3, n+1, 2):
        if i in prime:
            remove_elem = [i for i in range(2*i, n+1, i)]
            prime = [i for i in prime if i not in remove_elem]
    return len(prime)+1
```

## 문자열 정렬하기
> 입력받은 문자열 s를 알파벳 역순으로 정렬해보세요. 대문자는 소문자보다 뒤에 위치해야합니다.   
예시) Zbcdefg는 gfedcbZ가 됩니다.   

이 문제도 간단하다. sorted 함수를 사용해서 문자열을 정렬하고 그 순서를 뒤집으면 된다.

```
def solution(s):
    answer = "".join(sorted(s, reverse=True))
    return answer
```


### 참고
[위키백과: 에라토스테네스의 체](https://ko.wikipedia.org/wiki/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98_%EC%B2%B4)
