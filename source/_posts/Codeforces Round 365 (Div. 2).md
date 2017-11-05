---
title: Codeforces Round 365 (Div. 2)  题解
date: 2016-08-7 21:44:18
tags:
---
这场很坑，cf狂崩，提交的题目在几十分钟后才有回复，所以unrated。不过可能是因为这个原因，所以才勉强可以登得上去，不然在家里网络都不行。
<!--more-->

# A. Mishka and Game
time limit per test1 second
memory limit per test256 megabytes
inputstandard input
outputstandard output

### Description
Mishka is a little polar bear. As known, little bears loves spending their free time playing dice for chocolates. Once in a wonderful sunny morning, walking around blocks of ice, Mishka met her friend Chris, and they started playing the game.

Rules of the game are very simple: at first number of rounds n is defined. In every round each of the players throws a cubical dice with distinct numbers from 1 to 6 written on its faces. Player, whose value after throwing the dice is greater, wins the round. In case if player dice values are equal, no one of them is a winner.

In average, player, who won most of the rounds, is the winner of the game. In case if two players won the same number of rounds, the result of the game is draw.

Mishka is still very little and can't count wins and losses, so she asked you to watch their game and determine its result. Please help her!

### Input
The first line of the input contains single integer n n (1 ≤ n ≤ 100) — the number of game rounds.

The next n lines contains rounds description. i-th of them contains pair of integers mi and ci (1 ≤ mi,  ci ≤ 6) — values on dice upper face after Mishka's and Chris' throws in i-th round respectively.

### Output
If Mishka is the winner of the game, print "Mishka" (without quotes) in the only line.

If Chris is the winner of the game, print "Chris" (without quotes) in the only line.

If the result of the game is draw, print "Friendship is magic!^^" (without quotes) in the only line.

### Examples
input
`3
3 5
2 1
4 2`
output
`Mishka`
input
`2
6 1
1 6`
output
`Friendship is magic!^^`
input
`3
1 5
3 3
2 2`
output
`Chris`

### Note
In the first sample case Mishka loses the first round, but wins second and third rounds and thus she is the winner of the game.

In the second sample case Mishka wins the first round, Chris wins the second round, and the game ends with draw with score 1:1.

In the third sample case Chris wins the first round, but there is no winner of the next two rounds. The winner of the game is Chris.

这题意思是有n局掷骰子游戏，每一局可能有胜负平，整个比赛也有可能正负平(根据获胜的次数来决定)。然后就模拟咯。

```cpp
#include <stdio.h>
#define MAX 2
#define ll long long
using namespace std;
int main()
{
    int n,m,c,a=0,b=0;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        scanf("%d%d",&m,&c);
        if(m>c)
            a++;
        else if(m<c)
            b++;
    }
    if(a>b)
        printf("Mishka\n");
    else if(b>a)
        printf("Chris\n");
    else
        printf("Friendship is magic!^^\n");
    return 0;
}
```
***

# B. Mishka and trip
time limit per test1 second
memory limit per test256 megabytes
inputstandard input
outputstandard output

### Description
Little Mishka is a great traveller and she visited many countries. After thinking about where to travel this time, she chose XXX — beautiful, but little-known northern country.

Here are some interesting facts about XXX:

XXX consists of n cities, k of whose (just imagine!) are capital cities.
All of cities in the country are beautiful, but each is beautiful in its own way. Beauty value of i-th city equals to ci.
All the cities are consecutively connected by the roads, including 1-st and n-th city, forming a cyclic route 1 — 2 — ... — n — 1. Formally, for every 1 ≤ i < n there is a road between i-th and i + 1-th city, and another one between 1-st and n-th city.
Each capital city is connected with each other city directly by the roads. Formally, if city x is a capital city, then for every 1 ≤ i ≤ n,  i ≠ x, there is a road between cities x and i.
There is at most one road between any two cities.
Price of passing a road directly depends on beauty values of cities it connects. Thus if there is a road between cities i and j, price of passing it equals ci·cj.
Mishka started to gather her things for a trip, but didn't still decide which route to follow and thus she asked you to help her determine summary price of passing each of the roads in XXX. Formally, for every pair of cities a and b (a < b), such that there is a road between a and b you are to find sum of products ca·cb. Will you help her?

### Input
The first line of the input contains two integers n and k (3 ≤ n ≤ 100 000, 1 ≤ k ≤ n) — the number of cities in XXX and the number of capital cities among them.

The second line of the input contains n integers c1, c2, ..., cn (1 ≤ ci ≤ 10 000) — beauty values of the cities.

The third line of the input contains k distinct integers id1, id2, ..., idk (1 ≤ idi ≤ n) — indices of capital cities. Indices are given in ascending order.

### Output
Print the only integer — summary price of passing each of the roads in XXX.

### Examples
input
`4 1
2 3 1 2
3`
output
`17`
input
`5 2
3 5 2 2 4
1 4`
output
`71`

这题意思是给定一个环，然后给定几个环上的几个点为省会城市，省会城市和环上其他所有点都有连线。求这样的一张图每条路的value的和，路的value定义为两个端点的value乘积。这题刚开始看的时候想了一会后来觉得O(N)就可以做了。首先算出环上的所有值，然后对于不是省会城市的城市来说，连线会来自相邻的两个点(算过了，所以后面要去掉)和其他省会城市点(不包含相邻点)。对于是省会城市的点来说，会有来自其他省会城市的连线(这里要注意单独拿出来算sum，因为在最后要除以2)和相邻点(算过了)的连线。

```cpp
#include <stdio.h>
#define maxn 100005
#define ll long long
using namespace std;
ll c[maxn];
bool hashs[maxn];
int main()
{
    int n,k,tmp;
    ll sum=0,sum2=0,ans=0,ans2=0;
    scanf("%d%d",&n,&k);
    for(int i=0;i<n;i++)
    {
        scanf("%I64d",&c[i]);
        if(i!=0)
            ans+=c[i]*c[i-1];
    }
    ans+=c[0]*c[n-1];
    for(int i=0;i<k;i++)
    {
        scanf("%d",&tmp);
        hashs[tmp-1]=true;
        sum2+=c[tmp-1];
    }
    for(int i=0;i<n;i++)
    {
        if(hashs[i]==false)
        {
            ans+=c[i]*sum2;
            if(hashs[((i-1)<0?n-1:i-1)]==true)
                ans-=c[i]*c[((i-1)<0?n-1:i-1)];
            if(hashs[(i+1)%n]==true)
                ans-=c[i]*c[(i+1)%n];
        }
        else
        {
            ans2+=c[i]*(sum2-c[i]);
            if(hashs[((i-1)<0?n-1:i-1)]==true)
                ans2-=c[i]*c[((i-1)<0?n-1:i-1)];
            if(hashs[(i+1)%n]==true)
                ans2-=c[i]*c[(i+1)%n];
        }
    }
    printf("%I64d\n",ans+ans2/2);
    return 0;
}
```

***
cf崩的不行了，做完这两题就快结束了，后面的两题难度也差的很多，所以就暂时没有补了。

