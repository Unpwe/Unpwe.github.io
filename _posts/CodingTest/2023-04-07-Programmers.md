---
title: "[프로그래머스] [LV.2] 광물 캐기"
layout: single
categories: Cpp
author_profile: true
sidebar_main: true
tag: Cpp, CodingTest
toc: true
---



CodingTest
{: .notice--warning}



## 문제

![image](https://user-images.githubusercontent.com/69719507/230490088-1d7ab72a-4336-40dc-8a87-8fca9ac4f1d9.png)


이 문제는 광석에 따라 각기 다른 피로도를 가지고 있는 3가지의 곡괭이를 이용하여 캘수있는 광석 리스트를 받아와서 해당 광석을 가장 효율좋게 캐야하는 마인크래프트가 생각나는 문제이다.


<br>



## 해결방식


이 문제의 핵심은 **[어떤 곡괭이든 최대 5개까지의 광석들만 캘 수 있다.]** 이다.   
이 소리는 즉 광석을 5개를 캐면 해당 곡괭이는 사라지고 다른 곡괭이를 꺼내야 하고, 또한 중간에 바꿀수 없으며 **[광물은 반드시 주어진 순서대로만 캘 수 있다.]**   

때문에 가장 최고의 경우의 수만 고르는 [그리디 알고리즘]을 적용하려면 그대로의 광석리스트를 사용하기에는 무리가 있다.   
왜냐하면 그리디 알고리즘의 경우 이번에 선택한 최고의 경우의 수가 다음 번의 최고의 경우의 수에 영향을 미치는 것을 고려하지 않기 때문이다.   

해당 문제의 경우 곡괭이를 언제 어디에 쓰냐에 따라 최대 피로도가 달라지니 조금 변형을 해주어야 했다.   

때문에 가장 최악의 경우의 수인 [돌 곡괭이]로 캘 때의 피로도를 계산하여 피로도가 큰 순서대로 내림차순으로 정리하여 해당 리스트를 토대로 그리디 알고리즘을 적용하는 방식으로 해결했다.   


<br>



## 코드

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

struct Node
{
    int IndexCnt;
    int Fatigues;
    Node(int a, int b)
    {
        IndexCnt = a;
        Fatigues = b;
    }
    
    bool operator<(const Node& Other) const
    {
        return Fatigues < Other.Fatigues;
    }
    
};

int MineralValue(int Pick, string mineral)
{
    if(mineral == "diamond")
    {
        switch(Pick)
        {
            case 0:
                return 1;
            case 1:
                return 5;
            case 2:
                return 25;
        }
        
    }
    if(mineral == "iron")
    {
        switch(Pick)
        {
            case 0:
                return 1;
            case 1:
                return 1;
            case 2:
                return 5;
        }      
    }
    if(mineral == "stone")
    {
        switch(Pick)
        {
            case 0:
                return 1;
            case 1:
                return 1;
            case 2:
                return 1;
        }       
    }
}

int solution(vector<int> picks, vector<string> minerals) {
    int answer = 0;    
    
    /* 각 곡괭이의 갯수 */
    int D = picks[0];
    int I = picks[1];
    int S = picks[2]; 
    
    /* 광물의 갯수와 곡괭이의 갯수는 다르다. */
    int Value = minerals.size() < (D + I + S) * 5 ? minerals.size() : (D + I + S) * 5;
    
    /* 다이아, 철 , 돌 곡괭이 피로도 리스트 */
    vector<vector<int>> Fatigues(3, vector<int>((Value / 5) + 1, 0));

    /* 내림차 순으로 돌 곡괭이 기준으로 가장 피로도가 높은 것으로 저장 */
    priority_queue<Node> FatiguesList;
        
    int DiaSum = 0;
    int IronSum = 0;
    int StoneSum = 0;
    
    for(int IndexCnt = 0, i = 1; i <= Value; i++)
    {
        DiaSum += MineralValue(0, minerals[i - 1]);
        IronSum += MineralValue(1, minerals[i - 1]);
        StoneSum += MineralValue(2, minerals[i - 1]);
        if(i % 5 == 0 || i == minerals.size())
        {
            Fatigues[0][IndexCnt] = DiaSum;
            Fatigues[1][IndexCnt] = IronSum;
            Fatigues[2][IndexCnt] = StoneSum;
            FatiguesList.push(Node(IndexCnt, StoneSum));
            DiaSum = 0;
            IronSum = 0;
            StoneSum = 0;
            IndexCnt++;
        }
    }
    
    while(!FatiguesList.empty())
    {
        Node node = FatiguesList.top();
        FatiguesList.pop();
 
        if(D > 0)
        {
            answer += Fatigues[0][node.IndexCnt];
            D--;
        }
        else if(I > 0)
        {
            answer += Fatigues[1][node.IndexCnt];
            I--;
        }           
        else if(S > 0)
        {
            answer += Fatigues[2][node.IndexCnt];
            S--;
        }            
    }
       
    return answer;
}

```

***


> 이 문제의 핵심은 결국 돌 곡괭이로 캤을때 피로도가 높을 수록 가장 좋은 곡괭이로 캐야 효율이 좋다.


코드가 좀 길어졌지만, 구조체와 광석마다 return 해야할 피로도를 계산해주는 함수를 제외하면 실 코드는 그리 길지는 않았다.   

한 곡괭이로 5개의 광석을 캐야하니 광석 리스트를 5개씩 묶음으로 피로도를 계산해서 각 곡괭이들로 치환했을때의 피로도를 곡괭이별 피로도 리스트에 저장하였다.   

5로 나누어서 [IndexCnt]를 기반으로 피로도 배열에 저장하니 결국 [IndexCnt]가 1씩 증가할 수록 그 다음 5개의 광석의 피로도인 셈이다.   

의외로 중요 한 것이 메인 코드의 첫 부분에 광석 리스트와 곡괭이 갯수를 비교해서 더 적은것으로 피로도 리스트를 초기화 하고 for문을 돌리는 것이였다.    

곡괭이가 최대 5개까지 캘수있다는건 꼭 5개가 아니더라도 반드시 캐야 하는 것이다.   
광석이 22개라면 5로 나누고 마지막 2개까지의 피로도도 계산하여야 한다.   

하지만 곡괭이가 광석에 비해 모자르다면 얘기가 달라진다. 광석은 반드시 **[순서대로]** 캐야 하기 때문에, 이것을 무시하고 광석 갯수를 기준으로 피로도를 저장한다면 우선순위로 인해 순서대로 절대 올수없는 마지막 2개 캤을때 피로도를 계산하거나 하는 등으로 문제에 어긋나게 된다.   
(그럼에도 불구하고 테스트 케이스에서 1개만 틀려서 좀 의아하긴 했다.)   


구조체 [Node]의 역할은 5개씩의 피로도와 해당 광석들의 **[위치 인덱스값]** 을 가지고 있게 하였다.   
> 결국 돌 곡괭이로 캔 리스트를 참고하여 높을 수록 다이아 곡괭이나, 철 곡괭이로 치환 해주어야 하는데 그냥 피로도만 priority_queue에 저장하면 어디 인덱스인지를 모르기 때문이다.   

그 다음 priority_queue에 있는 것들을 빼와 먼저 나올수록 반드시 캤을 때 피로도가 큰 광석 5개 이므로 해당 광석들의 위치 값인 [IndexCnt]를 보고 좋은 곡괭이로 치환해주며 더해주면 된다.



<br>



## 해결


![image](https://user-images.githubusercontent.com/69719507/230494217-66e48779-9d21-4cdb-b1f1-010431b0857d.png)


***
