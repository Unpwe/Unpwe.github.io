---
title: "[알고리즘][C++] Union & Find"
layout: single
categories: Data Structure
author_profile: true
sidebar_main: true
tag: [Algorhythm, Cpp, Data Structure]
published: true
---

알고리즘
{: .notice--success}


## Union & Find

**나뭇잎이 떨어지면 뿌리로 돌아간다.**<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;***- 낙엽귀근(落葉歸根)***
{: .notice--warning}

서로 다른 그룹을 가진 두 원소가 존재할 때 **두 원소를 하나의 그룹으로 만들어줄 수도있고, 두 원소가 같은 그룹인지 아닌지도 판단할 수 있는 알고리즘이다.**   
간단한 로직으로 그룹화를 시켜 같은 그룹인지 아닌지 판단하거나 **서로 다른 그룹을 한 그룹으로 묶어줄 수있기 때문에** 자료구조형태로 사용할 수 있다.


## 해결방식

> **"재귀적 탐색"**

서로 다른 그룹을 하나의 그룹으로 묶는 것은 간단하다.   
그냥 그룹을 나타내는 배열에 그룹 숫자만 바꿔주면 되기 때문이다.   

**그렇다면 두 그룹이 서로 다른 그룹인지 같은 그룹인지 판단할 때는 어떨까?**    
[A섬]과 [B섬] 사이에 다리를 건설하고 [C섬]과 [D섬] 사이에도 다리를 건설했다고 치자.   
시간이 지나 [B섬]과 [C섬] 사이에도 다리가 건설되었다면, **[A섬]과 [D섬]은 이제 같은 그룹인지 어떻게 판단할까?**   


**Union & Find** 알고리즘은 **여러 그룹을 하나로 합치고, 두 원소가 같은 그룹인지 빠르게 확인할 수 있는 알고리즘이다.**


<br/>


## 코드구현

```cpp
#include <iostream>
#include <vector>

using namespace std;

int Find(vector<int>* union_list, int num)
{
    if(num == (*union_list)[num])
        return num;
    else
        return (*union_list)[num] = Find(union_list, num); // 핵심 코드. 재귀를 리턴하면서 Union 리스트를 갱신함.
}



void Union(vector<int>* union_list, int n1, int n2)
{
    int group1 = Find(union_list, n1);
    int group2 = Find(union_list, n2);

    if(group1 != group2)
        (*union_list)[n1] = n2;
}


int main() 
{
    int group_cnt;
    cout << "그룹의 갯수 입력 : ";
    cin >> group_cnt;

    vector<int> union_list(group_cnt + 1);

    for(int i = 0; i <= group_cnt; i++)
        union_list[i] = i; // 각 그룹별로 번호 지정
    
    int input_cnt;
    cout << "입력할 갯수 입력 : ";
    cin >> input_cnt;

    int num1, num2 = 0;
    for(int i = 0; i < input_cnt; i++)
    {
        int num1= 0, num2 = 0;
        cout << "원소 2개 입력 : ";
        cin >> num1 >> num2;

        Union(&union_list, num1, num2);
    }

    for(int i = 0; i <= group_cnt; i++)
        cout << i << "번은 " << union_list[i] << "그룹\n";
    
    return 0;
}
```

<br/>

## 결과
![image](/assets/images/알고리즘/Union&Find결과.png)

> **return (*union_list)[num] = Find(union_list, num);**

**재귀적으로 union_list(각 원소의 그룹을 나타내는 리스트)를 갱신하며 동시에 return.**   

Union 함수는 간단하다.   
원소를 넣어서 나온 **Find함수로 나온 그룹이 서로 다른 경우** union_list에 그룹을 바꿔주면 된다.   

> **Find 함수의 핵심은 해당 그룹의 그룹장을 찾는다는 것이다.**   
이때 return과 동시에 union_list에 저장을 해주면 이미 찾아보았던 원소를 또 다시 재귀를 돌면서 찾아가지 않아도,   
찾아내는 과정에서 **루트 그룹장**으로 갱신이 되기때문에 또 **선형적으로 그룹을 찾아가지 않아도 된다.**
{: .notice--success}


<br/>

각 원소가 그룹을 가지고 있고, 이것을 병합하거나 해제하거나 같은 그룹인지 판단해야할 때 사용할 수 있는 **자료구조로서 유용한 알고리즘이다.**


