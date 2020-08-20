---
title: 그래프 (2)
categories: 
    - Develop
tags:
    - algorithm
    - java
    - baekjoon
---


## Minimum Spanning Tree(MST)
undirected weight graph에서 weight 합이 최소인 spanning tree
(spanning tree: graph의 모든 vertex를 포함하는 tree)

### MST 찾는 방법

1. Kruskal's algorithm

1. weight 기준으로 오름차순 정렬
2. weight가 작은 edge 순서대로 선택
3. 선택한 edge가 cycle을 만들면 제외
4. 모든 vertex를 포함할 때까지 반복 

- cycle 여부 체크 방법 : union-find 이용
선택된 edge의 양끝 vertex가 같은 집합에 있으면 cycle로 간주

2. Prim's algorithm

## Loweat Common Ancestor(LCA)

두 vertex의 ancestor중 depth가 가장 깊은 ancestor

### LCA 찾는 방법

1. DFS(BFS)를 이용하여 각 vertex의 depth와 parent vertex를 찾는다
2. depth가 일치하게 맞춘다.
3. 역으로 하나씩 올라가면서 LCA를 찾는다.

- 두 vertex의 depth 차이가 엄청 크다면 하나씩 올라가는 것은 시간 낭비
-> 각 vertex의 parent 뿐만 아니라 2^k번째 ancestor까지 저장(Dynamic Programming)

어떤 vertex V의 2^k번째 ancestor인 vertex를 parent[K][V]에 저장

parent[K][V] = parent[K-1][parent[K-1][V]]

두 vertex의 parent[K][V] 값이 K=k일 때 같아진다면 LCA는 2^(k-1) ~ 2^k번째 ancestor 사이에 존재한다



벨만-포드 알고리즘
