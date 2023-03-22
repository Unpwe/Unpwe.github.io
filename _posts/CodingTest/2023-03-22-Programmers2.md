---
title: "[프로그래머스] [LV.2] 숫자 변환하기"
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

![Programmer](https://user-images.githubusercontent.com/69719507/226934815-cbe234da-0a3a-4f38-98bd-e83cf6fd301b.png)


이 문제는 자연수 [x]에 [n]을 더하거나 2를 곱하거나 3을 곱해서 결국 최종적으로 [y]를 얼마만에 만들수 있는가? 아니면 못만드는가? 를 물어 보는 문제이다.

***


## 해결방식

이 문제의 경우 딱봐도 모든 경우의 수를 계산하여 그 중 가장 적은 횟수를 나타내는 문제 이므로 DFS나 BFS가 떠올랐다.   

처음에는 DFS로 작성 해볼까 했지만 BFS 메모제이션 방식도 나쁘지 않을 것 같아 해당 방식으로 채택했다.

***


## 코드


```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

struct Node
{
    int num;
    int cnt;
    
    Node(int a, int b)
    {
        num = a;
        cnt = b;
    }
};

int solution(int x, int y, int n) {
    
    if(x == y)
        return 0;
    
    queue<Node> q;
    vector<bool> mem(y+1, false);
    
    int cnt = -1;
    q.push(Node(x, 0));
    
    while(!q.empty())
    {
        Node node = q.front();
        q.pop();
        
        if(mem[node.num])
            continue;
        
        mem[node.num] = true;    
        if(node.num + n == y || node.num * 2 == y || node.num * 3 == y)
        {
            cnt = node.cnt + 1;
            break;
        }
        
        if(node.num * 3 < y)
            q.push(Node(node.num * 3, node.cnt + 1));
        if(node.num * 2 < y)
            q.push(Node(node.num * 2, node.cnt + 1));
        if(node.num + n < y)
            q.push(Node(node.num + n, node.cnt + 1));    
    }
  
    return cnt;
}
```
***


이 방식의 핵심 중점은 **메모제이션** 배열인 bool형 vector배열 [mem]이다.   
이 문제에서 x가 y를 넘어 버리는 순간 x가 그 어떤 값을 더하고 곱해도 의미가 없으므로 y만큼의 크기를 가진 하나의 **방문 방명록**을 만들면 된다.

구조체 [Node] 경우 값과 얼마나 더해졌는지 세는 [cnt]를 가지고 있다.

> **도착한 모든 값들은 vector\<bool>mem에 출석체크를 해야한다.**

* 현재 풀이 방식에 가장 중요한 핵심이자, 이 방명록에 유무에 따라 속도차이가 어마무시하게 난다.

* 만약 어디선가 값이 1004가 들어와서 해당 1004에 [+ n / *2 / *3]을 하고 값을 큐에 넣어놨다고 가정해보자.   
연산하다가 갑자기 어느곳에서 1004가 한번더 튀어 나온다면 또 다시 1004 값에 [+ n / *2 / *3]을 해서 큐에 넣어질 것이다.   
이렇게 되면 큐 대기줄만 길어질 뿐만 아니라, 애초에 BFS에서는 먼저 온 값이 반드시 cnt가 낮으므로 연산하는 의미가 없다.

* 때문에 어떤 값이 들어왔을때 해당 값은 이미 큐에 넣어놨다는 표시를 해두고 다음에 어디선가 해당 값이 또 등장하면 바로 기각해버린다.



> **queue\<node>에 들어간 값들은 아직 y까지 도달하지 못한 값들이다.**

* queue에 들어간다는 것은 결국 이미 가득 서 있는 대기 줄 가장 뒷부분에 서는 것이다.   
때문에 이미 답이 코앞에 있는데도 의미없이 대기열에 줄서라고 보내버리는 것보다, 애초에 값을 더해서 결과를 보고 아니다 싶으면 cnt + 1 하면서 큐에 넣는 방식이다.

* 만약 [+ n / *2 / *3] 경우의 수중에 하나라도 목표 타겟인 y가 된다면 BFS에서는 가장 먼저 빨리 발견한 것이 곧 최단 경로 이므로 바로 break 해준다.


***


## 해결


### 메모제이션을 한 경우

***

![Programmer](https://user-images.githubusercontent.com/69719507/226942107-6b7a920c-ea5c-467b-bf18-a2ea8a7cf52d.png)

**속도도 빠르고 잘 되는 것을 볼 수 있다.**


### 메모제이션을 하지 않은 경우

***


![Programmer](https://user-images.githubusercontent.com/69719507/226942595-50583b53-ff5b-4f64-8e5d-ad48816b23d8.png)


**속도 차이가 어마무시하게 나면서 아예 시간초과가 나버리는 경우를 볼 수 있다.**


