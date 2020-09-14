---
title: 기계 번역(Machine Translation)
categories: 
    - Develop
tags:
    - deep learning
    - nlp
    - machine translation
---
***

딥러닝을 기반으로 한 NLP 프로젝트를 진행하게 되었다.   
뉴스 기사에 어울리는 제목을 딥러닝을 사용해서 생성하는 것이 이번 프로젝트의 목표다.   
구글 번역, 네이버 파파고에서 사용하는 machine translation을 사용하여 문제를 해결하기로 했다.   
우선 machein translation이 무엇인지 살펴보자.   

## Machine Translation

> Machine translation은 컴퓨터가 하나의 언어로 된 텍스트(source text)를 다른 언어(target text)로 변환하는 기술을 말한다.   

Machine translation 기술은 다음과 같이 발전했다.   

1. Rule-based
2. Statistical(SMT)
3. Neural(NMT)   

## Rule-Based Machine Translation   

영어를 한글로 번역하는 가장 단순한 방법은 영단어를 한글로 바꾸는 것이다.   
영한사전만 있으면 가능한 방법이지만 *문법이나 문맥을 전혀 고려하지 않기 때문에* 좋은 결과를 얻을 수 없다.   

## Statistical Machine Translation(SMT)

NMT가 등장하기 전까지 machine translation은 SMT 기법을 적용했다.   
SMT를 사용하려면 우선 많은 데이터가 필요하다.   
데이터는 parallel corpus 형태이며 이를 사용하여 가능한 많은 후보군을 만든 다음 정확성을 평가하여 순위를 매긴다.   
즉, 정확성이 가장 높은, 정답에 가까운 확률을 가진 후보를 선택하는 방식이다.   

예를 들어, I don't go home.이라는 문장을 한글로 번역해보자.   

나는 집에 가지 않는다.   

Rule-based로 번역했다면 나 아니다 가다 집. 수준의 결과가 나올 것이다.   
예시를 한 문장만 들었지만 SMT를 사용할 때는 많은 양의 데이터를 사용한다.   
I를 한글로 번역하면 나, 나는, 내가 등 많은 경우의 수가 존재할 수 있다.   

다음과 같은 표가 나올 수 있다.   

|I|don't|go|home| 
|:===:|:====:|:===:|:===:|:===:|:===:|:===:|
|나|않는다|가다|집|
|나는|아니다|간다|집에|
|내가|안|가지|집으로|
   
<br>
예시는 단어를 단위로 번역했지만 굳이 단어일 필요는 없다.   
단어가 될 수도 있고 구가 될 수도 있다.    
여하튼 후보들 중에서 확률값이 가장 높은 것들을 찾아나가며 번역하는 과정을 거친다.  

이 방법도 여러 문제가 있다.   
구 이상의 범위에서 나타나는 언어적 특성을 반영하기 어렵고 번역 단위를 연결할 때 매끄럽지 못할 수 있다.   

## Neural Machine Translation(NMT)

딥러닝이 등장하면서 mahcine learning 기술은 급속도로 발전했다.   
NMT는 이러한 구조를 갖고 있다.   
<br>   
![NMT](https://i.imgur.com/0sxat55.png)
<br>   

그림에서 input layer = I don't go home.이고 output layer = 나는 집에 간다. 이다.  
그럼 중간에 hidden layer는 무엇일까?   

바로 **문장 벡터**이다.   

우리가 색깔을 표현할 때 빨강, 노랑처럼 이름을 말할 수도 있지만 RGB 값을 사용하여 (0,0,0), (255,255,255)같은 벡터로 표현할 수도 있다.   

단어도 이처럼 벡터로 표현할 수 있다.   
이를 단어의 분산 표현(Distributed Representation)이라고 한다.   

그럼 단어를 어떻게 벡터로 표현할 수 있을까?   

여기서 **분포 가설(Distributional Hypothesis)**가 등장한다.   

> 비슷한 문맥을 가진 단어는 비슷한 의미를 갖는다.   

강아지라는 단어를 생각해보자. 강아지는 어떤 단어와 잘 어울릴까?   
귀엽다, 사랑스럽다가 떠오른다.   
그렇다면 '강아지'라는 단어는 귀엽다, 사랑스럽다와 같은 문맥을 갖게 된다.   

Distributional hypothesis가 어떤 느낌인지 감이 왔다.   
그렇다면 얘를 어떻게 표현하고 사용하는걸까?   

크게 두 가지 방법이 있다.   

1. Count : 단어, 문맥의 출현 횟수를 카운팅
2. Prediction : 단어에서 문맥, 문맥에서 단어를 예측

이쯤에서 확실히 알아야하는 것은 단어 자체의 의미는 고려하지 않는 것이다.   

중요한 것은 **문맥**이다.   

I don't go home.과 I don't like home.이라는 문장에서 <u>go와 like 자체 의미는 상관없이 같은 문맥을 지니고 있다.</u>라고 보는 것이 distributional hypothesis의 핵심이다.   

Count-based method와 predictive method에 대한 설명도 적으면 좋지만 글이 너무 길어지기 때문에 나중에 따로 정리하기로 하고.   

다시 NMT로 돌아오자.   

그림을 보면 input text는 vector로 바뀌고 이 vector가 output text로 바뀌는 구조이다.   

input text를 vector로 바꾸는 부분을 **encoder**, vector를 output text로 바꾸는 부분을 **decoder**라고 한다.   
Encoder와 decoder가 바로 neural network로 구성되어 있다.   
Encoder로 만들어진 벡터는 rule-based, SMT와 달리 input text의 전체 정보를 담고있기 때문에 자연스러운 결과를 만들어낼 수 있다.   

따라서 encoder와 decoder에 의해 성능이 좌우된다.   

이번 프로젝트에서는 ~~(아직 확실하지 않지만)~~ seq2seq model을 사용할 것이다.   
seq2seq 모델이 무엇인지는 다른 글에서 알아보자.   
이제 막 NMT를 공부하는 참이라 많은 글을 읽어야한다.   

마지막으로 앞으로 알야야할 것 같은 중요한 개념을 나열해보자.   
아마 여기서 언급되는 용어들에 대해 글을 쓸 것 같다.   

- count-based
- predictive
- word2vec
- word embedding
- CBOW
- softmax regression
- loss function
- coverage loss
- overfitting
- hyperparameter
- seq2seq
- RNN, LSTM, GRU
- CNN
- GPT
- BERT
- attention
- parallel corpus mining
- back translation
- BLEU score
- transformer

## 참고   

Young, T., Hazarika, D., Poria, S., & Cambria, E. (2017). Recent Trends in Deep Learning Based Natural Language Processing.   

Peter F., John C., Stephen A., Vincent J., Fredrick J. John D., Robert L., Paul S., (1990). A statistical approach to machine translation.   

Ilya S., Oriol V., Quoc V., (2014), Sequence to sequence learning with nerual networks.   

[유원준. 2020. 딥러닝을 이용한 자연어 처리 입문](https://wikidocs.net/book/2155)   

[밑바닥부터 시작하는 딥러닝](https://velog.io/@dscwinterstudy/%EB%B0%91%EB%B0%94%EB%8B%A5%EB%B6%80%ED%84%B0-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-%EB%94%A5%EB%9F%AC%EB%8B%9D-2%EC%9E%A5-94k64m2939)   














