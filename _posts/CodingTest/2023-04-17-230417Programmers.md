---
title: "[프로그래머스] [LV.2] 두 원 사이의 정수 쌍"
layout: single
categories: Cpp
author_profile: true
sidebar_main: true
tag: Cpp, CodingTest
toc: true
published: true
---





CodingTest
{: .notice--warning}



## 문제

![image](https://user-images.githubusercontent.com/69719507/232452695-770800f7-14ae-42c3-b28b-c463988dfaf4.png)

이 문제는 데카르트 좌표상에서 어떤 r1, r2반지름을 가진 두 원의 사이에 존재하는 점(좌표)의 갯수를 찾는 문제이다.

> 예시 [r1 : 2] / [r2 : 3]
![image](https://user-images.githubusercontent.com/69719507/232453104-458a29c8-0311-4477-8b74-b1164c31fe89.png)


<br>



## 해결방식


이 문제의 경우 아무리봐도 수학적인 문제인 것같아서 고민을 많이하다가 반지름을 알려주고 좌표이다보니 아무래도 피타고라스를 이용해서 풀어야 하는 문제라고 생각했다.  

피타고라스의 [밑변의 제곱 + 높이의 제곱 = 빗변의 제곱]를 이용해서 지금 우리가 알고있는 빗변(반지름)을 이항을 하여 [빗변의 제곱 - 밑변의 제곱 = 높이의 제곱]을 유추 할수 있고, 해당 높이는 결국 점의 갯수라는 방식으로 풀었다.



<br>


## 코드



```cpp
#include <string>
#include <vector>
#include <cmath>

using namespace std;

long long solution(int r1, int r2) {   
    long long result = 0;  
    long long r2Length = (long long)r2 * r2;
    long long r1Length = (long long)r1 * r1;
    
    for(int i = 1; i <= r2; i++)
    {
        long long r2Point = 0;
        long long r1Point = 0;
        long long X = (long long)i * i;
        
        r2Point = sqrt(r2Length - X);       
        if(i < r1)
            r1Point = ceil(sqrt(r1Length - X));
                 
        result += (r2Point - r1Point) + 1;
    }   
    
    return result * 4;
}

```


> **이 문제에서 내가 간과한 점은 다름아닌 [형 변환] 이였다.**

반지름의 제곱은 달라질 일이 없으므로 미리 계산해두고 진행했는데 계속 테스트 케이스에서 앞문제는 맞지만 뒷 문제는 틀렸다.   
왜 일까 고민하면서 사례를 만들어보니 형 변환이 제대로 이루어 지지 않고 쓰레기값으로 변해있었다.

<br>

> **큰 원은 내림, 작은원은 올림을 해주어야한다.**

높이의 길이가 반드시 정수가아니라 [2.23606797749979...] 이런식으로 소숫값으로 나오기때문에 내리거나 올려주어야한다.

이때 큰 원의 경우 정수가 의미하는 것은 Y가 0인 경우를 제외한 점의 갯수이고,   
작은 원의 경우 또한 점의 갯수지만 작은원의 경우는 Y가 0일때 좌표상에 찍히는 점까지 세주기 위해 올림을 해주었다.   

<br>

> **작은 원에서 높이값이 정수값으로 딱 떨어지면 원이 정확하게 좌표상에 걸쳐있는 것이다.**

이 문제의 경우 원이 결쳐져 있는 부분 또한 점으로 인정하기 때문에, 소숫값이아닌 정수로 딱 나눠지는 경우 또한 큰 원과 작은 원 사이에 존재하는 점으로 세줘야한다.  

때문에 작은 원의 경우 소숫점에서 올림을 해주는 함수인 [ceil]을 사용하면, 높이가 소숫값인 경우, 정수 값인 경우, 두 경우 다 해결이 가능하다.    




<br>



## 해결


![image](https://user-images.githubusercontent.com/69719507/232457413-25a3a647-8622-4834-ae87-94b1f18fd9a5.png)


수학 문제는 어렵다. 많이 접해보는 수밖에 없을 것 같다.



***