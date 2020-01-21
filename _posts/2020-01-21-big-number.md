---
layout: post
title:  "프로그래머스 코딩테스트 연습 - 큰 수 만들기"
date:   2020-01-21 12:22:00 +0900
categories: Python Programmers Level_2
---

<br />

## Contents
1. 문제 설명 <br /> 
[제한사항] <br />
[입출력 예] <br /><br />
3. 알고리즘 분석 <br />
[나의 풀이]<br />
[Most 1 의 풀이]

<br /><br />

## 문제 설명 
어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.

예를 들어, 숫자 1924에서 수 두 개를 제거하면 [19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.

문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다. number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.

<br />

#### 제한사항
-   number는 1자리 이상, 1,000,000자리 이하인 숫자입니다.
-   k는 1 이상  `number의 자릿수`  미만인 자연수입니다.

<br />

#### 입출력 예 
| number | k | return |
|:----|:----:|:----:|
|'1924'| 2 | '94' |
|'1231234'| 3 | '3234' |
|'4177252841'| 4 | '775841' |


<br /><br />

## 알고리즘 분석
 이번에는 자력으로 풀지 못했다. 시도는 해봤으며, 실행까지는 무사히 통과했으나 코드를 체점하니 오류가 나왔다. 
 모든 경우의 수를 조사하는 방법도 있었으나 시간초과로 통과하지 못했다. 

#### Most 1의 풀이
```python
def solution(number, k):
    stack = [number[0]] 
    for num in number[1:]: # 숫자를 비교하면서 숫자가 작으면 스택에 있는 숫자를 버린다. 
        while len(stack) > 0 and stack[-1] < num and k > 0:
            k -= 1 
            stack.pop() # 스택에서 숫자를 꺼낸다.
        stack.append(num) # 스택에 숫자를 집어넣는다.
    if k != 0:  # 모든 숫자가 동일할 경우. 
        stack = stack[:-k]
    return ''.join(stack)
```
`stack = [number[0]] `
stack에 number의 첫번째 인덱스에 있는 값을 대입한다. 
`for num in number[1:]:`
number의 두번째 인덱스에 있는 값부터 차례대로 num에 대입하면서 반복문을 실행.
`while len(stack) > 0 and stack[-1] < num and k > 0:` 스택에 요소가 한개 이상 있고, 스택의 마지막 요소보다 num의 값이 크고, k가 0이상이면 다음 코드를 실행한다.
`k -= 1`
`stack.pop()`
스택의 요소를 제거하고, k를 -1하여 조건에 맞게 큰 수를 만든다.
`stack.append(num)` 또한 위의 반복문과는 별개로 스택에 반드시 현재 num에 있는 숫자를 추가한다. 
`if k != 0: stack = stack[:-k]`
이 코드는 모든 숫자가 동일할 경우, number에 전혀 변화가 없기 때문에 만들었다. 
`return ''.join(stack)`  마지막으로 리스트인 스택의 요소들을 문자열로 반환한다. 

<br /><br />

#### Most 2의 풀이 
```python
def solution(number, k):
    st = []
    for i in range(len(number)):
        while st and k > 0 and st[-1] < number[i]:
            st.pop()
            k -= 1
        st.append(number[i])
    return ''.join(st[:len(st) - k])
```
이 코드는 Most 1의 풀이와 거의 비슷하다. 
`st = []`  Most 1은 number의 첫번째 인덱스의 숫자를 대입한 것과 다르게 아무것도 대입하지 않았다. 하지만 어차피 `st.append(number[i])`로 인해 숫자가 들어가기 때문에 같다고 볼 수 있다. 
또한 Most 1에서는 if문으로 구현한 코드 또한 
`''.join(st[:len(st) - k])`으로 join에 한번에 구현한 것을 볼 수 있다. k가 0이라면 st의 모든 요소가 문자열로 바뀔 것이고 k가 1보다 크다면 뒤의 요소가 일부 빠진 상태로 문자열로 바뀌어 반환될 것이다. 

<br /><br /><br />
