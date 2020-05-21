---
title: Heteronym 찾기
categories: 
    - Develop
tags:
    - nlp
    - nltk
    - python
    - heteronym
---
***

Nlp 수업 세 번째 과제는 **heteronym 찾기**다.   
중간고사가 취소돼서 과제 수가 많아지고 비중도 훨씬 커졌다.   
Heteronym 찾기뿐만 아니라 몇 가지 기준에 따라 **ranking**도 해야하는 과제다.

## Heteronym이란?

Merriam-Webster Dictionary에 의하면 heteronym은 "one of two or more homographs (such as a bass voice and bass, a fish) that differ in pronounciation and meaning"이다.
> 철자는 같지만 발음과 의미가 다른 단어가 heteronym이다.   

Heteronym은 같은 POS를 가질 수도 있고 아닐 수도 있다. 어떤 단어가 heteronym이냐면 bass(물고기, 낮은 음), tear(눈물, 찢다), address(주소, 연설하다) 등이 있다.   
Heteronym 과제 목표는 heteronym이 최소 두 번 나타나는 문장과 해당하는 heteronym의 **pronouncing annotation**을 찾는 것이다. 

## Ranking 기준

Heteronym만 찾으면 되는 게 아니라 csv로 output을 뽑을 때 찾은 문장을 ordering해서 상위 30개 문장을 찾는 것이 최종 목표다. Ranking 기준은 다음과 같다.
<br>
1. Homograph가 많이 있을수록 순위가 높다.
- wind + wind + tear + tear > wind + wind + tear
2. Homograph가 있는 문장이 heteronym만 있는 문장보다 순위가 높다.
- wind + wind > wind + tear
3. POS가 같은 heteronym을 가진 문장이 순위가 더 높다.
- tear + tear > PROduce + proDUCE

## Question

요약하면 heteronym이 있는 문장을 찾으면 되는건데 description을 읽으면서 생긴 질문을 몇개 적어본다.
<br>
1. Heteronym이 있으면 무조건 뽑는다?   
2. 특정 단어가 heteronym인지 아닌지 automatically하게 찾아야하는건가?
3. Heteronyms but not homographs가 정확히 어떤 경우일까?

## Answer

Office hour에서 조교님들한테 물어봤다.
첫 번째로, heteronym를 automatic한 방법으로 찾으려면 의미와 발음이 다른 두 단어가 한 문장안에 있어야하기 때문에 pair로 존재할 때만 찾으면 된다.   
따라서 automatically하게 heteronym을 찾으면 된다. 
세 번째 질문의 대답을 통해 문제가 더 명확해졌다.    **Heteronym은 object 개념이고 homograph는 relation 개념이다.**

위 개념을 가지고 ranking criteria를 다시 살펴보자.
>wind + wind + tear + tear > wind + wind + tear      

왼쪽은 wind pair와 tear pair 총 두 개의 homograph가 있고 오른쪽은 wind pair, 하나의 homograph가 있어서 왼쪽 순위가 더 높다.

>wind + wind > wind + tear    
  
왼쪽은 wind pair, 하나의 homograph가 있지만 오른쪽은 wind 하나 tear 하나가 있어서 사실 homograph가 없다.

## 앞으로 할 것

지금도 정확히 이해한 건 아니다. Heteronym, homograph, ranking criteria의 정확한 정의는 조만간 다시 공지해주신다니 잠시 기다려야겠다.   
그 전에 할 일은 무작위 corpora 중에 heteronym이 많이 나타날 것 같은(말장난 위주 text)를 찾는 것부터 구현해야겠다.