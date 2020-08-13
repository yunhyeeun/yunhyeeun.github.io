---
title: Data Structure
categories: 
    - Develop
tags:
    - data structure
    - java
    - baekjoon
---

> Data structure는 효율적으로 접근하고 수정할 수 있도록 데이터를 구성하고 저장하는 방법을 말한다.   

선형 구조 : 데이터가 일렬로 나열   
    array, linked list, stack, queue

비선형 구조 : 데이터가 특정한 형태를 띄고 있음   
    tree, graph

1. array
- 접근이 쉬움
- insert/delete가 어려움

2. linked list
- 접근이 느림
- insert/delete가 쉬움
- prev/next에 대한 포인터 필요

3. stack
    - 나중에 삽입된 데이터가 먼저 삭제되는 구조 (LIFO)
    - top 위치에 새로운 데이터를 삽입

4. queue
    - 먼저 삽입된 데이터가 먼저 삭제되는구조 (FIFO)
    - front(head) : 삭제될 위치
    - rear(tail) : 삽입된 위치

5. tree
    - parent-child relationship
    - root node : 최상위 노드
    - depth : root에서 해당 노드까지 도달하는데 사용하는 간선의 개수
    - height : root에서 해당 노드까지 도달하는 지나간 정점의 개수 (보통 가장 큰 값을 의미)


## Binary tree
모든 노드의 child 개수가 2개 이하(left child, right child)

* array로 binary tree 구현 가능

### Class로 tree 구현하기
```
class Node {
    Object data
    Node left_child, right_child
}
```
### Array로 tree 구현하기
1. root node를 array[1]에 저장
2. 임의의 parent node의 index를 p라고 하자
3. left child의 index는 2p
4. right child의 index는 2p+1


### Binary tree iteration

1. pre-order : current node -> left child -> right child

2. in-order : left child -> current node -> right child

3. post-order : left child -> right child -> current node

## Heap

heap : complete binary tree + (parent node value > child node value) or (parent node value < child node value)

### Heap Insert

1. 마지막 위치에 insert
2. 추가한 node와 부모를 비교
3. heap 조건에 맞춰 swap
4. heap 조건에 만족하거나 추가한 node가 root에 도달할 때까지 반복

### Heap Delete

1. root 삭제
2. 마지막 node를 root로 이동
3. root와 child를 비교
4. heap 조건에 맞춰 swap
5. heap 조건에 만족하거나 leaf node에 도달할 때까지 반복

## Priority Queue

단순 queue가 아니라 특정 조건에 따라 data 별로 priority를 부여한 다음 queue에서 poll할 때 priority가 가장 높은 data를 꺼냄

**우선순위를 heap condition으로 가지는 heap으로 구현**

## Indexed Tree

data를 tree의 leaf node로 지정한 후 각각의 parent node는 child의 합이나 count 등등 요구조건에 맞춰 정의

**perfect binary tree이기 때문에 부족한 leaf node에 대한 초기값을 설정해야 함**

보통 특정 행동을 반복 수행하는데 중간에 행동 결과가 달라지는 행동을 할 때 사용하면 편하다.   
(예. array 구간합을 구하는데 중간에 array value가 바뀔 때)

## Indexed Tree Create

1. leaf node 개수가 N개 이상이 되도록 높이가 T인 tree array를 만든다. (배열 크기 : 2^T)
2. leaf node에 data 추가 (array idx : 2^(T-1) ~ N-1)
3. array idx 2^(T-1) - 1부터 1 순서대로 left child & right child을 참조하여 data 추가

## Indexed Tree Query

예시. 구간합 문제
이 때 indexed tree는 parent가 child의 합으로 이루어져있다고 가정   
1. root에서 시작하여 pre-order iteration 진행
2. current node가 원하는 구간에 완전히 속해있으면 current node data return
3. (left child data + right child data) return

## Indexed Tree Update

1. node data update
2. update parent data

## Trie

K진 tree (tree node child 개수가 2보다 큰 K개)
- 문자열을 빠르게 검색할 수 있는 구조
- root는 항상 공백 상태

### TrieNode
 class Node {
     Object data
     Node child[]
 }

### Search Trie

1. root부터 시작하여 찾는 단어의 첫 글자부터 탐색
2. current node의 child 중 해당하는 글자가 있다면 아래로 이동
3. 그렇지 않다면 존재하지 않는 단어로 처리


