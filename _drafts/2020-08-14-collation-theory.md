---
title: 조합론
categories: 
    - Develop
tags:
    - 
    - java
    - baekjoon
---

순열(permutation) : N개 중 K개를 순서있게 고르는 경우의 수
중복순열(repeated permutation) : N개 중 중복을 허용하여 K개를 순서있게 고르는 경우의 수
조합(combination) : N개 중 K개를 순서 없이 고르는 경우의 수
중복조합(repeated combination) : N개 중 중복 허용하여 K개를 순서 없이 고르는 경우

파스칼의 삼각형

nCk = n-1Ck-1 + n-1Ck
(n개 중에 k를 뽑을 때, 하나를 기준으로 이것을 뽑은 경우와 뽑지 않은 경우로 나누면 됨)