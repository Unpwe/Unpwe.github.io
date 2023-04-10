---
title: "[프로그래머스] [LV.2] 연속된 부분 수열의 합"
layout: single
categories: Python
author_profile: true
sidebar_main: true
tag: Python, CodingTest
toc: true
---





CodingTest
{: .notice--warning}



## 문제

![image](https://user-images.githubusercontent.com/69719507/230967915-0569019b-5025-4b09-b4fa-354257a468eb.png)

이 문제는 비내림차순(오름차순)으로 정렬된 수열에서 합이 k인 수열중 가장 길이가 짧은 인덱스를 찾아내는 것이다.



<br>



## 해결방식


이 문제의 경우 수열자체는 연속적이므로 다이나믹 느낌으로 2차원 배열을 선언해서 풀어보려했으나 시간이 너무 많이 걸릴 것같아서, 1차원 배열의 다이나믹 배열로 풀기로 하였다.    

문제 자체가 딱히 설명할 것없이 구해야 하는 것이 명료해서 결국 **[인덱스의 위치]** 에 집중하여 문제를 해결하기로 하였다.

다이나믹 배열에 들어가있는 요소는 해당 인덱스까지의 더했을때의 값을 저장했고, 이 값은 첫 시작을 가리기는 Start라는 변수마다 다르게 설정하였다.   



<br>


## 코드



```python
def solution(sequence, k):
    AnswerStart = 0
    AnswerEnd = len(sequence)
    
    dy = [0 for _ in range(AnswerEnd)]
    
    dy[0] = sequence[0]
    if dy[0] == k:    
        return [0, 0]
     
    Start = 0
    for i in range(1, len(sequence)):
        num = dy[i - 1] + sequence[i]
        if num < k:
            dy[i] = num
        elif num == k:
            if AnswerEnd - AnswerStart > i - Start:
                AnswerStart = Start
                AnswerEnd = i               
            dy[i] = num - sequence[Start]
            Start+=1
        else:
            while(num >= k):
                num = num - sequence[Start]
                Start+=1
                if num < k:
                    dy[i] = num
                    break
                elif num == k:
                    if AnswerEnd - AnswerStart > i - Start:
                        AnswerStart = Start
                        AnswerEnd = i

    return [AnswerStart, AnswerEnd]

```


결국 배열이 오름차순으로 정렬되어있기 때문에, 계속 더해갈때 더한 값이 [k] 보다 큰 경우는 더이상 그 수열은 계산을 하면 안된다.   

때문에 더한 값이 [k]보다 작아 질 때까지 [Start] 변수의 위치를 하나씩 늘려가는 투포인터의 느낌으로 풀었다.

다이나믹 배열에 저장되어있는 그 수열의 총 합 에서 해당 인덱스의 값을 빼주면 결국 해당 인덱스가 빠진 수열의 합이기 때문에 이점을 이용 하였고

계속 갱신중인 끝 - 시작 인덱스의 값을 비교해보고 이번에 구한 값이 작으면 바꿔주었다.   




<br>



## 해결


![image](https://user-images.githubusercontent.com/69719507/230971498-597c9623-c800-434e-b3c3-ab668f8c4c12.png)


파이썬의 다른 기능들을 이용해보면 더 편하게 작성 할 수 있을거 같은데, 아직 C++스타일이 더 익숙한 것 같다. 더 공부 해야겠다.



***