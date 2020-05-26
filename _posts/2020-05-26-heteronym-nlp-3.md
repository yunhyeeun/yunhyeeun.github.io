---
title: Heteronym 과제 끝
categories: 
    - Develop
tags:
    - nlp
    - nltk
    - python
    - heteronym
---

Heteronym 과제를 끝냈다. 중간 과정을 포스팅하지 않아서 해결과정과 리뷰를 간략하게 요약하고 마무리해야겠다.

## 문제 설명 
> Heteronym이 있는 문장과, 각 heteronym에 맞는 발음 기호를 주어진 기준에 따라 정렬해 보세요.

무조건 지켜야하는 ranking 기준은 없지만 아래와 같이 ranking하면 좋다고 조교님께서 말씀해주셨다.

1. Heteronym 개수가 많은 순으로 우선 정렬
2. Heteronym 개수가 같을 경우
    - High: 표기는 같지만 발음이 다른 문장 (wind/wind/, wind/waind/)
    - Medium: 표기가 다른 문장 (wind, tear)
    - Low: 표기도 같고 발음도 같은 문장 (wind/wind/, wind/wind/)
3. 문장이 짧으면 or heteronym pos가 다르면 높은 순위

## 해결 과정

문제를 풀기 위해 크게 세 단계로 나누었다.

1. <u>Heteronym 찾기<u>
2. <u>Input text 찾기<u>
3. <u>Pronouncing annotation 찾기 & ranking하기<u>

### Step 1
이전 글을 쓸 때 step1을 한창 짜고 있었는데 외부 library를 사용해도 된다는 말을 듣고 급히 수정했다.   
<br>
기존 코드는 cmudict를 사용해서 multiple pronunciation word를 찾았다. 그 결과 5000개 정도의 단어를 찾았고 이 모든 단어가 heteronym이 아닌건 확실하기 때문에 조건을 추가해야했다.   
<br>
**Wordnet**을 사용해서 synonym set이 없거나 하나인 경우를 제외했다. 그리고 **Lemma**인 단어만 heteronym으로 생각하기로 했다.   
예를 들어 기본형인 rebel을 heteronym list에 추가했다면 변형인 rebels, rebeled, rebellion은 heteronym list에 추가하지 않기로 했다.   
이 과정은 Lemmatizer 함수를 사용해서 단어의 기본형을 찾고 추가로 suffix list를 만들어서 *suffix를 포함하고 있는 단어를 제외*하기로 했다.   
<br>
이제 1000개 단어가 남았고 이 단어를 **Wiktionary**에 검색해서 자세히 검토하기로 했다. 사실 처음부터 Wiktionary를 검색하면 되지만 그러면 run time이 너무 길어서 디버깅도 힘들고 채점하시기에도 좋지 못할 것 같아서 후보를 최대한 줄인 다음에 검색하기로 했다.
<br>
후보 단어의 <u>etymology가 여러개인지</u>, 여러개라면 각각의 발음 기호를 확인하고 다르면 heteronym인게 확실하다. 이 때 발음이 기호가 다른 것이 미국식 발음과 영국식 발음이어서 다른게 아니라 미국식 발음 내에서 다르다는 것을 의미한다.
<br>  
Etymology가 하나라도 POS가 다르고 <u>각 POS별로 발음이 다르게 될 수도 있으니</u> 그것도 확인한다. Wiktionary에서 발음에 대한 설명이 없는 경우도 있는데 이는 일반적인 단어가 아니거나, 고유명사이거나, 외국어 등의 이유여서 heteronym이 아닌 것으로 간주했다. Wiktionary 검색까지 다 하면 100 여개의 heteronym을 찾는다.   
<br>
다음 단계에서 이 긴 과정을 반복하기 싫어서 heteronym information은 csv file에 따로 저장했다.

### Step 2
Heteronym을 잘 찾았다 하더라도 input sentence에 heteronym이 없다면 좋은 결과를 만들 수 없다. 우리가 말을 할 때, 글을 쓸 때 heteronym을 일부러 많은 쓰는 경우는 언제일까? Heteronym은 철자는 같아도 발음과 뜻이 다른 단어이기 때문에 주로 유머 소재나 노래 가사 안에 많이 등장한다. 따라서, **reddit 사이트의 'humor'와 'wordplay' 카테고리 글**들을 불러오기로 했다.  
<br>
이 단계까지 와보니 nlp 수업을 들으면서 크롤링을 직접 해본 적이 없다는 걸 깨달았다. 주로 내장 corpora를 사용했는데, 새삼 수업을 헛으로 들었나 생각이 든다. Text preprocessing과 Beautifulsoup 관련 수업을 복습하고 코드를 짜기 시작했다.   
<br>
감사하게도 **praw API**라는 게 있어서 reddit 계정만 있으면 reddit submission을 긁어올 수 있었다. 그렇게 가져온 text에서 heteronym이 있는 문장을 찾는 코드를 살짝 돌려봤더니 생각보다 결과가 좋지 않았다.   
<br>
상위 rank 30개 문장을 output으로 받아왔는데 4개의 문장만 heteronym이 두 개 이상 나타났고 나머지는 전부 heteronym이 하나만 존재하는 문장이었다. Text를 더 많이 가져오면 되는데 문제는 praw가 가지고 올 수 있는 subreddit의 submission이 최대 1,000개였다. 그래서 **pushshift**를 사용하기로 했다.   
<br>
Pushshift는 reddit 특정 사이트만이 아니라 모든 사이트에 공통적으로 사용할 수 있는데, limit은 500개지만 게시글 작성 날짜 범위를 정할 수 있어서 범위를 바꿔가며 text를 계속 가져오면 무한정 가져올 수 있다.   
현재 날짜에서 1년 전까지 올라온 게시글을 input text로 설정하고 문장을 찾았더니 10개 문장이 두 개 이상의 heteronym을 가지게 되었다. 더 할 수도 있지만 역시나 run time이 길어져서 여기까지만 받아오고 다음 단계로 넘어가기로 했다.

### Step 3
이제 heteronym이 있는 문장을 찾았으니 알맞는 발음을 매칭해야한다. Cmudict은 pronouncing dictionary이긴 한데 pronunciation과 sense가 mapping된 사전은 아니어서 **Merriam-Webster online dictionary**를 사용하기로 했다. Wiktionary보다 가져오기가 편해서다.   
<br>
heteronym에 발음을 매칭하려면 이 heteronym이 문장 안에서 어떤 의미로 쓰이는지를 분석해야한다. 그런데 의미를 분석하는 방법은 정말 아이디어가 안 떠오르고(머신러닝을 쓰지 않는 선에서) 제출기한도 얼마 남지 않아서 **POS**로 구분하기로 했다.   
"Sow"는 '씨앗을 뿌리다'라는 동사와 '암퇘지'라는 명사, 두 개의 뜻을 가지고 있는데 둘의 발음이 달라서 이러한 경우를 찾아내기로 했다. 찾은 문장을 POS tagging해서 heteronym의 POS를 확인하고 Merriam-Webster에서 heteronym을 검색한 다음 POS에 해당하는 발음을 넘겨주는 방식으로 코딩했다. 

## 결과
당연한 결과겠지만 POS가 다른 heteronym은 발음 구분이 잘 된 반면, POS가 같은 heteronym은 하나도 구분이 되지 않았다. POS 말고도 pronounciation과 sense를 연결할 수 있는 다른 방법이 있을 듯 한데 다른 사람들은 어떻게 했는지 알고싶다.   
그리고 ranking은 사실 찾은 문장 자체에 heteronym이 그렇게 많지 않아서 중요한 문제가 아니었다. 그냥 heteronym이 많은 순으로 정렬하고 POS와 문장의 길이를 확인하고 재정렬했다.

