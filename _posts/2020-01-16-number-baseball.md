---
layout: post
title:  "프로그래머스 코딩테스트 연습 - 숫자 야구"
date:   2020-01-16 15:00:00 +0900
categories: Python Programmers Level_2
---

<br />

## Contents
1. 문제 설명 
[제한사항]
[입출력 예]
3. 알고리즘 분석 
[빠리님의 풀이]
[Most 1 의 풀이]

<br /><br />

## 문제 설명 
&nbsp;&nbsp;숫자 야구 게임이란 2명이 서로가 생각한 숫자를 맞추는 게임입니다. 

&nbsp;&nbsp;각자 서로 다른 1~9까지 3자리 임의의 숫자를 정한 뒤 서로에게 3자리의 숫자를 불러서 결과를 확인합니다. 그리고 그 결과를 토대로 상대가 정한 숫자를 예상한 뒤 맞힙니다.

* 숫자는 맞지만, 위치가 틀렸을 때는 볼
* 숫자와 위치가 모두 맞을 때는 스트라이크
* 숫자와 위치가 모두 틀렸을 때는 아웃

예를 들어, 아래의 경우가 있으면

```
A : 123
B : 1스트라이크 1볼.
A : 356
B : 1스트라이크 0볼.
A : 327
B : 2스트라이크 0볼.
A : 489
B : 0스트라이크 1볼.
```

이때 가능한 답은 324와 328 두 가지입니다.

질문한 세 자리의 수, 스트라이크의 수, 볼의 수를 담은 2차원 배열 baseball이 매개변수로 주어질 때, 가능한 답의 개수를 return 하도록 solution 함수를 작성해주세요.

<br />

#### 제한사항
-   질문의 수는 1 이상 100 이하의 자연수입니다.
-   baseball의 각 행은 [세 자리의 수, 스트라이크의 수, 볼의 수] 를 담고 있습니다

<br />

#### 입출력 예 
|baseball|return|
|:---------------------------------------|:--:|
|[[123,1,1],[356,1,0],[327,2,0],[489,0,1]]| 2 |

<br /><br />

## 알고리즘 분석
&nbsp;&nbsp;이번 문제는 어떻게 해야할지 감이 안와서 구글링을 통해 코드를 찾았는데, 정말 괜찮은 코드인거 같아서 가져와봤다. 

```python
from itertools import permutations

def check_score(question, candidate, s, b):
    strike = 0
    for i in range(len(question)):
        if question[i] == candidate[i]:
            strike += 1
    if s != strike:
        return False
    ball = len(set(question) & set(candidate))-strike
    if b != ball:
        return False
    return True

def solution(baseball):
    lst = list(permutations([1,2,3,4,5,6,7,8,9], 3))
    for i in baseball:
        for item in lst[:]: #item들이 후보들
            if not check_score([int(i) for i in list(str(i[0]))], item, i[1], i[2]):
                lst.remove(item)
    return len(lst)
```
[출처] = 빠리의 택시 운전사
[https://geonlee.tistory.com/116](https://geonlee.tistory.com/116)

`lst = list(permutations([1,2,3,4,5,6,7,8,9], 3))`
`permutations()` 는 순열을 구하는 외장 함수이다. 
해당 외장함수를 이용하여, [1,2,3,4,5,6,7,8,9] 안에 있는 숫자들 중 3개를 가지고 만들 수 있는 모든 조합을 list 형식으로 만든후 lst에 저장한다. 

단, 이 때 [1,1,2] 처럼 중복된 숫자가 있는 경우는 제외된다.

`for i in baseball:` 
[[123, 1, 1], [356, 1, 0], [327, 2, 0], [489, 0, 1]] 가 입력 값이라면?

1. i = [123, 1, 1]
2. i = [356, 1, 0]  
3. i = [327, 2, 0]
4. i = [489, 0, 1] 

순차적으로 위처럼 대입된다.  

 `for item in lst[:]:` 
이것 또한 마찬가지로 다음과 같이 대입된다.
1. item = (1,2,3)
2. item = (1,2,4)
3. ...
4. item = (9,8,7)

근데 여기서 lst 가 아니라 lst[:] 를 사용한 이유는 무엇일까? 

그 이유는 lst[:] 는 lst 리스트 자체가 아니라 그것을 카피한 또다른 객체이기 때문이다. 즉, lst가 변경되더라도 lst[:]에는 변화가 없다. 자세한 이유는 밑에서 설명하겠다.

`if not check_score([int(i) for i in list(str(i[0]))], item, i[1], i[2]):`
check_score() 라는 함수가 거짓을 반환 할 때, `lst.remove(item)`를 실행하는 코드이다. 

`def check_score(question, candidate, s, b):` 를 한번 살펴보자. 

함수에 매개변수가 대입된다면 다음과 같이 될 것이다. 
- check_score([1,2,3],(1,2,3), 1, 1] 
`strike = 0`  스트라이크를 나타내는 변수를 0으로 초기화
`for i in range(len(question)):` question의 길이는 항상 3이다.
`if question[i] == candidate[i]:` 두 숫자의 같은 자리의 수를 비교하여 같으면 `strike += 1` 를 실행한다.     

`if s != strike:` s 와 strike가 서로 틀리면 `return False`을 반환한다. 그 이유는 
1. condidate = (2,1,3) 일 경우
baseball = [[1,2,3], 1, 1]
strike = 0, s = 1 
즉, (2,1,3)은 답이 될 가능성이 없다. 
False를 반환 하였기 때문에 `lst.remove(item)`가 실행되어 lst에서 삭제된다. 

2. condidate = (3,2,8) 일 경우 
baseball = [[1,2,3], 1, 1]
strike = 1, s = 1 
즉, (3,2,8)은 답이 될 가능성이 있다. 

`ball = len(set(question) & set(candidate))-strike`

1. condidate = (3,2,8) 일 경우 
ball = len( {1,2,3} & {8, 2, 3} ) - 1 
ball = len( {2,3} ) - 1 = 1  

`if b != ball: return False` 위의 스트라이크 개수를 확인하는 것과 같이 답이 될 가능성이 없는 lst 요소를 제거한다.

`return True` 답이 될 가능성이 있는 요소들은 True를 반환하여 삭제되지 않고 넘어간다.

<br /><br />

#### Most 1의 풀이 
&nbsp;&nbsp;이 풀이는 어떤 의미로 굉장해서 한 번 가져와봤는데, 효율적이진 않지만 경우의 수를 모두 계산해버리는 끈기는 인정할 수 밖에 없을 것 같다.
굳이 분석하지는 않겠다... 

```python
def solution(b):
    num = [str(i[0]) for i in b]
    result = [i[1:] for i in b]
    pos = [i for i in [str(i) for i in range(100,1000)] if (len(set(i))==3) and ('0' not in i)]
    s = set(pos)
    for i in range(len(b)):
        if result[i] == [0,0]:
            temp = set([j for j in pos if num[i][0] not in j and num[i][1] not in j and num[i][2] not in j])
            s = s&temp
        if result[i] == [0,1]:
            temp = set([j for j in pos if num[i][0] in j and num[i][1] not in j and num[i][2] not in j and j[0]!=num[i][0] and j[1]!=num[i][1] and j[2]!=num[i][2]])|\
            set([j for j in pos if num[i][1] in j and num[i][0] not in j and num[i][2] not in j and j[0]!=num[i][0] and j[1]!=num[i][1] and j[2]!=num[i][2]])|\
            set([j for j in pos if num[i][2] in j and num[i][0] not in j and num[i][1] not in j and j[0]!=num[i][0] and j[1]!=num[i][1] and j[2]!=num[i][2]])
            s = s&temp
        if result[i] == [0,2]:
            temp = set([j for j in pos if num[i][0] in j and num[i][1] in j and j[0]!=num[i][0] and j[1]!=num[i][1] and j[2]!=num[i][2]])|\
            set([j for j in pos if num[i][0] in j and num[i][2] in j and j[0]!=num[i][0] and j[1]!=num[i][1] and j[2]!=num[i][2]])|\
            set([j for j in pos if num[i][1] in j and num[i][2] in j and j[0]!=num[i][0] and j[1]!=num[i][1] and j[2]!=num[i][2]])
            s = s&temp
        if result[i] == [0,3]:
            temp = set([j for j in pos if num[i][0] in j and num[i][1] in j and num[i][2] in j])
            s = s&temp
        if result[i] == [1,0]:
            temp = set([j for j in pos if j[0]==num[i][0] and num[i][1] not in j and num[i][2] not in j])|\
            set([j for j in pos if j[1]==num[i][1] and num[i][0] not in j and num[i][2] not in j])|\
            set([j for j in pos if j[2]==num[i][2] and num[i][0] not in j and num[i][1] not in j])
            s = s&temp
        if result[i] == [1,1]:
            temp = set([j for j in pos if j[0]==num[i][0] and j[2]==num[i][1] and num[i][2] not in j])|set([j for j in pos if j[0]==num[i][0] and j[1]==num[i][2] and num[i][1] not in j])|\
            set([j for j in pos if j[1]==num[i][1] and j[2]==num[i][0] and num[i][2] not in j])|set([j for j in pos if j[1]==num[i][1] and j[0]==num[i][2] and num[i][0] not in j])|\
            set([j for j in pos if j[2]==num[i][2] and j[0]==num[i][1] and num[i][0] not in j])|set([j for j in pos if j[2]==num[i][2] and j[1]==num[i][0] and num[i][1] not in j])
            s = s&temp
        if result[i] == [1,2]:
            temp = set([num[i][0]+num[i][2]+num[i][1], num[i][2]+num[i][1]+num[i][0], num[i][1]+num[i][0]+num[i][2]])
            s = s&temp
        if result[i] == [2,0]:
            temp = set([j for j in pos if j[0]==num[i][0] and j[1]==num[i][1] and num[i][2] not in j])|\
            set([j for j in pos if j[0]==num[i][0] and j[2]==num[i][2] and num[i][1] not in j])|\
            set([j for j in pos if j[1]==num[i][1] and j[2]==num[i][2] and num[i][0] not in j])
            s = s&temp
        if result[i] == [3,0]:
            temp = set(num[i])
            s = s&temp
    return len(s)
```

<br /><br /><br />
