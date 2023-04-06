---
title: "[프로그래머스] [LV.2] 리코쳇 로봇"
layout: single
categories: cpp
author_profile: true
sidebar_main: true
tag: Cpp, CodingTest
toc: true
---


CodingTest
{: .notice--warning}


## 문제

![image](https://user-images.githubusercontent.com/69719507/229309598-087305b1-bab1-44d1-8b41-0dfffa9aa228.png)


이 문제는 일직선으로 쭉 이동하다가 D나 끝을 만나면 멈추고, 멈췄을 때 해당 칸이 G일때를 찾는 문제이다.


<br>


## 해결방식

이 문제의 경우 **[최단 거리]**를 찾는 문제이므로 최단 미로 탐색과 동일하게 BFS를 생각하였다.

중요한 쟁점은 미로 탐색과는 다르게 ["D나 배열의 끝이 오기 전까지 일직선으로 계속해서 이동하기"] 인 점이 가장 큰 다른점이다.

모든 칸에서 경우의 수를 계산해야하는 미로와는 다르게 멈추면 안되는 곳은 무시하고 계속 넘겨야 하기 때문이다. (종료지점인 G도 마찬가지)


<br>


## 코드

```cpp
#include <string>
#include <vector>
#include <queue>

using namespace std;

int dy[4] = { 1, 0, -1, 0 };
int dx[4] = { 0, 1, 0, -1 };

struct Node
{
    int y;
    int x;
    int cnt;

    Node(int a, int b, int c)
    {
        y = a;
        x = b;
        cnt = c;
    }

    Node()
    {
        y = 0;
        x = 0;
        cnt = 0;
    }
};

int solution(vector<string> board) {

    queue<Node> Q;
    vector<vector<bool>> Visit(board.size(), vector<bool>(board[0].size(), false));
    Node Start;

    bool bFlag = false;
    for (int i = 0; i < board.size(); i++)
    {
        for (int j = 0; j < board[0].size(); j++)
        {
            if (board[i][j] == 'R')
            {
                Start = Node(i, j, 0);
                bFlag = true;
                break;
            }
        }
        if (bFlag)
            break;
    }

    Q.push(Node(Start.y, Start.x, 0));
    Visit[Start.y][Start.x] = true;

    while (!Q.empty())
    {
        Node node = Q.front();
        Q.pop();

        for (int i = 0; i < 4; i++)
        {
            int yy = node.y;
            int xx = node.x;

            while ((yy >= 0 && yy < board.size() && xx >= 0 && xx < board[0].size()) && board[yy][xx] != 'D')
            {
                yy += dy[i];
                xx += dx[i];
            }

            if (yy < 0 || yy >= board.size() || xx < 0 || xx >= board[0].size())
            {
                yy -= dy[i];
                xx -= dx[i];
                
                if (board[yy][xx] == 'G')
                    return node.cnt + 1;

                if (Visit[yy][xx] == false)
                {
                    Q.push(Node(yy, xx, node.cnt + 1));
                    Visit[yy][xx] = true;
                }
                continue;
            }
            else if (board[yy][xx] == 'D' && Visit[yy - dy[i]][xx - dx[i]] == false)
            {
                if (board[yy - dy[i]][xx - dx[i]] == 'G')
                    return node.cnt + 1;

                Q.push(Node(yy - dy[i], xx - dx[i], node.cnt + 1));
                Visit[yy - dy[i]][xx - dx[i]] = true;
                continue;
            }          
        }
    }

    return -1;
}
```

> 사실 이 문제의 경우 시간이 조금 걸렸다..    
왜냐하면 처음엔 각 열에서 D의 위치만을 가지고 있는 2차원 vector를 가지고 각 열마다 D의 위치를 기준으로만 계산해서 푸는 방식이 좀 더 연산량이 줄어들어서 좋지않을까 하고 코드를 풀었다.    
하지만 내가 만들어본 반례는 다 통과하였음에도 답안을 제출하면 다른 곳에서 실패를 했다..   
그래서 그냥 단순하게 한 칸씩 넘기는 방식으로 풀기로 하였다.

***

<br>




> 이 문제의 핵심은 결국 멈춰서야 하는 곳까지 멈추지말고 저돌맹진을 해야한다.

D를 찾아내는건 쉽지만 멈추는 조건에는 해당칸이 배열의 끝인 경우도 있기 때문에, 저돌맹진을 하는 While문에서 조건을 **[해당칸이 끝이 아니거나 D가 아닌경우에 반복]** 하는 것으로 주었다.   

결국 상하좌우를 전부 확인 해야하기 때문에 코드 단순화를 위해서 방향을 더하는 [dy] [dx]를 만들어서 for문을 4번 돌리는 것으로 하였고,   
While문에서 이미 +[dy], +[dx]를 하고 빠져나왔기 때문에, 만약 배열 끝이여서 나온 상황이라면 
이미 배열의 인덱스를 벗어난 상황이다.   

때문에 반드시 한번은 빼주어야 다시 정상적은 배열인덱스로 돌아오기 때문에 -[dy], -[dx]를 해주었다.   


또한 중요한 것은 종료지점인 [G]를 만나더라도 멈추는것이 아니라 반드시 **[D의 전에 있거나, 배열의 끝]** 이여야 한다.    
그래서 D를 만났을 때와 배열의 끝일 때 멈춰야 하는 곳에 해당 칸이 [G]인지 판단하고 바로 cnt + 1를 return 해주는 방식으로 풀었다.


<br>



## 해결


![image](https://user-images.githubusercontent.com/69719507/229310374-b9495b04-0f5c-45ce-a173-96938ae11fec.png)


이 문제로 시행착오를 하면서 느낀점은, 극한의 효율로 풀어내고자 하는 방법보다는 알아보기 쉽게 풀어내는 것이 좋은 것 같다는 생각이 들었다.



***