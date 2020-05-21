---
title: Heteronym 과제 시작
categories: 
    - Develop
tags:
    - nlp
    - nltk
    - python
    - heteronym
---
***

조교님들이 Q&A에서 친절하게 문제를 설명해주셨다.   
문제를 재정의해보자.   

## Issues

- 구현해야하는 핵심 기능: heteronym 판별 + 의미에 맞는 발음 선택
-> 한 문장 안에 pair로 등장했을 떄와 그렇지 않을 때 발음 구분하는 것은 technically equivalent   
- Priority 기준: heteronym의 수가 많을수록(pair 여부와 상관없이), 같은 '표기인' heteronym이 동시에 등장하는 빈도가 높을수록 높은 ranking
- 같은 발음의 heteronym이 여러 번 반복할 경우 1번만 counting

## Approach

결국 한 문장 안에 'wind'가 한 번만 나타났더라도 counting을 해야하고 *'wind'가 '바람'인지  '(실을) 감다'인지* 판단할 수 있어야 한다.   
그렇다면 한 단어가 여러 발음으로 소리나는지 알면 heteronym 여부를 찾을 수 있지 않을까?   
Nltk에는 CMU Pronouncing Dictionary(cmudict)가 있어서 단어와 각각의 발음을 확인할 수 있다.   
단어가 갖고있는 발음의 종류가 여러개면 heteronym일 것이다. Heteronym을 구해보자.

```
import nltk

test = "conduct"
entries = list(nltk.corpus.cmudict.entries())
entries_dict = {}
for word, pron in entries:
    if word not in entries_dict:
        entries_dict[word] = [pron]
    else:
        entries_dict[word].append(pron)
multiple_pron_dict = { word: pron for word, pron in entries_dict.items() if len(pron) > 1 }
print (len(multiple_pron_dict.keys()))
```

Print 결과 9241개가 나왔다. Heteronym이 그만큼 많을리가 없다.   
그래서 key를 찍어봤더니 yesterday, house 등 heteronym이 아닌 단어까지 포함되었다.   
단순히 multiple pronounciation을 가진 단어 갯수만 세는 걸로는 안되나보다.   

문제에서 예시로 제시한 heteronym의 발음기호를 살펴보자.
>bass: ['B', 'AE1', 'S']    
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; ['B', 'EY1', 'S']   
wind: ['W', 'AY1', 'N', 'D']    
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;['W', 'IH1', 'N', 'D']   
conduct: ['K', 'AH0', 'N', 'D', 'AH1', 'K', 'T']   
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;['K', 'AA1', 'N', 'D', 'AH0', 'K', 'T']   
frequent: [['F', 'R', 'IY1', 'K', 'W', 'AH0', 'N', 'T']    
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ['F', 'R', 'IY1', 'K', 'W', 'EH2', 'N', 'T']]   

다음은 조교님께서 heteronym이 아니라고 언급한 단어들이다.   
>close: ['K', 'L', 'OW1', 'S']   
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ['K', 'L', 'OW1', 'Z']   
object: ['AA1', 'B', 'JH', 'EH0', 'K', 'T']   
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ['AH0', 'B', 'JH', 'EH1', 'K', 'T']   

두 그룹 모두 multiple pronounciation인데 하나는 heteronym이고 다른 하나는 heteronym이 아닐까?

## Vowel pronounciation

 
우리가 아는 알파벳과 달리 'AY1', 'IH1'처럼 이상한 기호가 발음 안에 있는 것을 확인할 수 있다.   
Cmudict은 [ARPABET](https://en.wikipedia.org/wiki/ARPABET)이라는 발음체계를 사용하는데 phoneme과 allophone을 나타내기 위해 'AY1'같은 2-letter code를 사용한다고 한다.   

그럼 숫자 1은 무엇을 나타내는 걸까?   
숫자는 강세의 유형을 나타낸다고 한다. 0이면 no stress, 1이면 primary stress 등등.   
Heteronym인 bass, wind, conduct, frequent 모두 **vowel pronounciation**에서만 차이가 난다.
반면 close는 consonant pronounciation에서만 차이가 난다.   
~~(조교님꼐서 object는 heteronym database에 따라 다르게 나타나서 크게 신경쓰지 말라고 하신다.)~~   
그렇다면 multiple pronounciation중에서 **vowel pronounciation이 다를때만** heteronym라고 가정하자.