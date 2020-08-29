---
title: Time complexity
categories: 
    - Develop
tags:
    - algorithm
    - java
    - baekjoon
---
<br>

알고리즘이 문제를 해결하는 방법이라면 시간복잡도는 문제를 해결하는데 걸리는 시간이다.   
사칙연산이나 조건문의 조건을 계산하는 횟수에 따라 실행시간이 결정된다.  
주로 반복문의 범위나 중첩 반복문에 의해 결정된다.  

## 시간복잡도

> 시간복잡도(time complexity)는 알고리즘의 수행시간이다.

### Time complexity 표기법

- big O : worst case
- big omega : best case
- big theta : average case

일반적으로 **big-O notation**을 사용한다.   
big-O로 표기할 때 *최고차항의 차수*만을 표기한다.   

### Time complexity 종류

- O(1)
- O(logN)
- O(N)
- O(NlogN)
- O(N^2)
- O(N^3)
- O(2^N)
- O(N!)

예1. Binary Search : O(logN)   

예2. Merge Sort : O(NlogN)   
분할(N) + 병합(NlogN)   

예3. 거듭제곱 빠르게 연산 : O(logN)    
지수를 2의 거듭제곱 형태로 나누어 계산   

### Time complexity 비교
N이 커질수록 차이가 커진다.   

|빠름| | | | | |느림|   
|:===:|:====:|:===:|:===:|:===:|:===:|:===:|
|1|logN|N|NlogN|N^2|2^N|N!|  

<br>
* * *

## 공간복잡도(Space Complexity)

공간복잡도는 알고리즘의 메모리 사용량에 관한 지표이다.

### 자료형 별 메모리 크기(java)

- 문자형(char) :  2byte
- 정수형(short) :  2byte
- 정수형(int) : 4byte
- 정수형(long) : 8byte
- 소수형(float) : 4byte
- 소수형(double) : 8byte

## 피보나치 계산

같은 문제여도 어떤 알고리즘을 사용하는지에 따라 time complexity, space complexity가 달라진다.   
피보나치 수열을 구하는 문제를 예시로 이를 알아보자.   

- 일반 재귀함수 : O(2^N)   
N부터 1까지 역으로 계산하는 방법   

```
func fibonacci(N) {
    if (N < 3) {
        return 1;
    }
    return fibonacci(N - 1) + fibonacci(N - 2);
}
```

- 반복문 : O(N)   
1부터 계산하는 방법   

```
func fibonacci(N) {
    int a = 1;
    int b =  1;
    int c = 1;
    for (int i = 2;i < N;i++) {
        c = a + b;
        a = b;
        b = c;
    }
    return c;
}
```

- dynamic programming : O(N)   
2번의 중간결과값을 저장 (미리 저장된 값의 경우 O(1))   

```
int[] dp = new dp[N + 1];
func fibonacci(N) {
    dp[1] = 1;
    dp[2] = 1;
    int a = 1;
    int b =  1;
    int c = 1;
    for (int i = 2;i < N;i++) {
        c = a + b;
        a = b;
        b = c;
        dp[i + 1] = c;
    }
    return c;
}
```

- 행렬의 거듭제곱 : O(logN)   


## [백준 2003번 수들의 합2](https://www.acmicpc.net/problem/2003)

문제 설명대로 수열의 부분합을 구하면 time complexity는 O(N^2)이다.   
**투포인터**를 사용하면 이를 줄일 수 있다.   
문제풀이에 핵심적인 요소를 정리해보았다.

- low index pointer와 high index pointer를 지정한다.
- 수열은 오름차순으로 정렬된 상태이다
- 부분합이 target보다 작으면 high pointer를 증가시킨다.
- 부분합이 target보다 크면 low pointer를 증가시킨다.
- high pointer가 범위를 벗어날 때까지 반복한다.

```
while (high < N) {
    // 부분합이 target보다 작을 때
    if (sum < M) {
        // high pointer 증가
        high++;
        if (high == N) break;
        sum += array[high];
    } else if (sum == M) {
        answer++;
        high++;
        if (high == N) break;
        sum += array[high];
    }
    // 부분합이 target보다 클 때
    else {
        sum -= array[low];
        // low pointer 증가
        low++;
        if (low > high) {
            high++;
            if (high == N) break;
            sum = array[high];
        }
    }
}

```

유사한 문제 [백준 1806번 부분합](https://www.acmicpc.net/problem/1806)
   

## [백준 2805번 나무자르기](https://www.acmicpc.net/problem/2805)

가장 높은 나무의 높이부터 0까지 차례대로 계산하면 O(N)의 time complexity로 문제를 해결할 수 있다.   
하지만 **binary search**를 사용하면 이를 O(logN)으로 줄일 수 있다.

- 배열은 정렬되어있다.
- 배열의 시작점(low)과 끝점(high)의 중간값 mid를 구한다
- target이 mid보다 작다면 high를 mid-1로 수정한다
- target이 mid보다 크다면 low를 mid + 1로 수정한다
- low가 high보다 커질 때까지 반복한다

```
while (low <= high) {
    mid = (high + low) / 2;
    sum = 0;
    for (int i = 0; i < N; i++) {
        sum += Math.max(trees[i] - mid, 0);
    }
    // sum이 M보다 큰 경우 -> low = mid + 1
    if (sum >= M) {
        low = mid + 1;
        answer = Math.max(answer, mid);
    }
    // sum이 M보다 작은 경우 -> high = mid - 1
    else {
        high = mid - 1;
    }
}
```

## [백준 2143번 두 배열의 합](https://www.acmicpc.net/problem/2143)

부분합 문제와 비슷하지만 배열이 두 개라는 차이점이 있다.   
완전탐색으로 풀려고 하면 time complexity가 O(N^4)정도 나오는데 다른 방법을 찾아보자.   
각 배열의 모든 부분합을 저장한 배열을 만들어 정렬하고 만들어진 두 부분합 배열을 고려하면 time complexity를 줄일 수 있다.   

1. 부분합 배열 만들어 정렬 : O(N^2)
2. 한 배열을 기준으로 투포인터를 사용하여 binary search로 계산 : O(NlogN)   

이 때 투포인터는 위 low, high가 아니라 lower bound&upper bound 개념을 말한다.   

- lower bound : 찾고자하는 값 이상이 처음 나타나는 위치   
- upper bound : 찾고자하는 값보다 큰 값이 처음으로 나타나는 위치   

다시 말해, 한 부분합 배열을 순회하면서(A[i]) 나머지 부분합 배열에서 target-A[i] 값을 count하면 된다.   
그리고 그 값은 **upper bound - lower bound**이다. 

```
subA = getSubarray(A);
subB = getSubarray(B);

Collections.sort(subB);

// 어차피 subA를 순회하기 때문에 subA는 sort할 필요가 없다
for (int i = 0;i < subA.size();i++) {
    long target = T - subA.get(i);
    answer += upperBound(target, subB) - lowerBound(target, subB);
}

public static int upperBound(long target, List<Long>array) {
    int low = 0;
    int high = array.size();
    while (low < high) {
        int mid = (low + high) / 2;
        // low, high를 수정하는 부분을 언제나 헷갈리니 유의하자
        if (subB.get(mid) <= target) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return low;
}

public static int lowerBound(long target, List<Long>array) {
    int low = 0;
    int high = array.size();
    while (low < high) {
        int mid = (low + high) / 2;
        // low, high를 수정하는 부분을 언제나 헷갈리니 유의하자
        if (subB.get(mid) < target) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }
    return low;
}

public static List<Long> getSubarray(int[] array) {
    List<Long> subarray = new ArrayList<Long>();
    for (int i = 0;i < array.length;i++) {
        long sum = 0;
        for (int j = i;j < array.length;j++) {
            sum += array[j];
            subarray.add(sum);
        }
    }
    return subarray;
}
```
이 문제에서 답이 int의  maxvalue를 초과하기 때문에 long으로 정의해야 통과한다.

## [백준 2517번 달리기](https://www.acmicpc.net/problem/2143)   

내 앞 사람 중 나보다 실력이 나쁜 사람의 수를 세는 문제이다.   
얼핏 보면 쉽지만 time complexity가 O(N^2)보다 작아야 통과할 수 있다.   
오래전에 배운거라 까먹었던 mergesort를 응용해야 풀 수 있는 문제다.   
(mergesort는 O(NlogN))이다.   
Mergesort를 요약해보자.   

1. divide (O(N))
배열의 원소를 하나씩 분리한다.
2. merge (O(NlogN))
인접한 원소를 비교하며 합친다.   

Divide 후 merge할 때, 그 중에 뒤 그룹의 원소를 합칠 때 앞 그룹에서 몇 개의 원소가 merge되었는지 count하면 된다.   
각 원소별로 계산한 누적 count를 처음 index에서 빼면 원하는 답을 구할 수 있다.

```
mergeSort(0, players.length - 1);

for (Player p : players) {
    answer[p.original - 1] = p.original - p.frontNum;
}

public static void mergeSort(int start, int end) {
    if (start < end) {
        int mid = (start + end) / 2;
        mergeSort(start, mid);
        mergeSort(mid + 1, end);
        merge(start, mid, end);
    }
}

public static void merge(int start, int mid, int end) {
    int frontPtr = start;
    int backPtr = mid + 1;
    int idx = start;
    int frontNum = 0;

    while (frontPtr <= mid && backPtr <= end) {
        if (players[frontPtr].capacity < players[backPtr].capacity) {
            frontNum++;
            Player p = players[frontPtr];
            tmp[idx++] = new Player(p.capacity, p.original, p.frontNum);
            frontPtr++;
        } else {
            Player p = players[backPtr];
            tmp[idx++] = new Player(p.capacity, p.original, p.frontNum + frontNum);
            backPtr++;
        }
    }
    while (frontPtr <= mid) {
        frontNum++;
        Player p = players[frontPtr];
        tmp[idx++] = new Player(p.capacity, p.original, p.frontNum);
        frontPtr++;
    }
    while (backPtr <= end) {
        Player p = players[backPtr];
        tmp[idx++] = new Player(p.capacity, p.original, p.frontNum + frontNum);
        backPtr++;
    }
    for (int i = start; i <= end; i++) {
        players[i] = tmp[i];
    }
}

class Player {
    int capacity;
    int original;
    int frontNum;

    public Player(int capacity, int original, int frontNum) {
        this.capacity = capacity;
        this.original = original;
        this.frontNum = frontNum;
    }

    @Override
    public String toString() {
        return "Player [capacity=" + capacity + ", frontNum=" + frontNum + ", original=" + original + "]";
    }
}
```

솔직히 mergesort 잊고있어서 도저히 못풀겠다 생각했던 문제다.   
mergesort말고 다른 방법이 있다는데 듣고도 까먹었다.   
이 방법 아니면 통과할 수 없어! 같은 느낌의 문제들은 많은 문제를 풀면서 다양한 경우에 익숙해지는 수 밖에 없다.






