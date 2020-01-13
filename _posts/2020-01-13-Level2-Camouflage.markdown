---
layout: post
title:  "프로그래머스 코딩테스트 연습 - 위장"
date:   2020-01-13 15:00:00 +0900
categories: Python Programmers Level_2
---

<br /><br /> 
**프로그래머스 (Programmers) 코딩테스트 연습 - 해시 - 위장** 
<br /><br /><br />

## 문제 설명

[출처] : [https://programmers.co.kr/learn/courses/30/lessons/42578] <br />
스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다. <br />
예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름 |
|--|--|
| 얼굴 | 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠 |
| 하의 | 청바지 |
| 겉옷 | 긴 코트 |

<br /><br />

### 제한사항
-   clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
-   스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
-   같은 이름을 가진 의상은 존재하지 않습니다.
-   clothes의 모든 원소는 문자열로 이루어져 있습니다.
-   모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
-   스파이는 하루에 최소 한 개의 의상은 입습니다.

<br /><br />

### 입출력 예

| clothes | return |
|--|--|
| [[yellow_hat,  headgear], [blue_sunglasses,  eyewear], [green_turban,  headgear]] | 5 |
| [[crow_mask,  face], [blue_sunglasses,  face], [smoky_makeup,  face]] | 3 | 

<br />

#### 입출력 예 설명

예제 #1  
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses

```

예제 #2  
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

```
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
```

<br /><br />

##  My Solution
```
def solution(clothes):
    ans = 1 
    new_clothe = [] # 옷을 종류별로 묶어서 리스트에 저장
    kind = []  # 옷의 종류를 저장 
    for clothe in clothes: # [옷의 이름, 옷의 종류]를 하나씩 꺼내서 반복문 실행 
        if clothe[1] in kind: # 옷의 종류가 kind안에 이미 존재할 경우 
           new_clothe[kind.index(clothe[1])].append(clothe[0]) # kind 안에서 '일치하는 옷의 종류의 인덱스'를 '옷의 이름'이 들어있는 리스트의 인덱스에 대입. 그 위치에 새로운 '옷의 이름'을 추가한다. 
        else: # kind안에 옷의 종류가 없을 경우 
            kind.append(clothe[1]) # 새로운 '옷의 종류'를 추가
            new_clothe = new_clothe + [[clothe[0]]] # '옷의 이름'을 새로운 이중 리스트에 추가
    for i in new_clothe:
        ans = ans * (len(i)+1)
        
    return ans-1
```
<br />

### 추가 설명 
> [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] 가 입력값으로 주어질 경우 
> - for clothe in clothes: 이 처음으로 실행되면
>
> [yellow_hat, headgear] 가 clothe의 값으로 오게된다. 
> - if clothe[1] in kind:  <그 다음으로 if 문이 실행> 
>
> headgear가 kind에 있는지 확인하지만 kind = []으로 None상태 이므로 
> else 문이 실행 
> - kind.append(clothe[1]) # 새로운 '옷의 종류'를 추가
       new_clothe = new_clothe + [[clothe[0]]]
> 
> kind = [headgear], new_clothe = [[yellow_hat]] 의 상태가 된다.
>
> 그 후 for문이 실행되며 같은 방식으로 값이 추가되어
> kind = [headgear, eyewear], new_clothe = [[yellow_hat],[blue_sunglasses]] 가 된다. 
>
> 마지막으로 for 문에서 if 문이 실행된다.
> - new_clothe[kind.index(clothe[1])].append(clothe[0]) 가 실행
> 
> 현재 clothe[1] = headgear 이고 kind에서 headgear라는 값은 인덱스 '0'에 위치해 있기 때문에 kind.index(clothe[1]) = 0 이 된다.
> 
> 다음과 같이 실행되어 
> new_clothe[0].append('green_turban')
> 
> kind = [headgear, eyewear]
> new_clothe = [[yellow_hat, green_turban], [blue_sunglasses]] 가 된다. 
>
> 마지막으로 가진 옷으로 조합할 수 있는 경우의 수를 찾아야 한다. 
>
> 만약 A = [a,b,c], B = [d,e] 가 있을 때, <br />
> 경우의 수를 구하려면 <br />
> 아무것도 고르지 않았을 경우를 0 이라고 가정하여 <br />
> A = [0,a,b,c], B = [0,d,e]  # A는 4개, B는 3개 가 된다. <br />
> 결국 4 * 3 = 12가지인데 [0, 0]처럼 아무것도 고르지 않았을 경우를 제외하여 12-1 = 11가지가 된다. <br /><br />
> 코드로 나타내면 다음과 같다.
> - for i in new_clothe:
> - ans = ans * (len(i)+1)
> - return ans-1
