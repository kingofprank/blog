---
title: Educational Codeforces Round 15 题解
date: 2016-08-01 21:31:11
tags:
---
在家CF上不去，点开了A题写完后网页就崩了，只能赛后补。好在是edu场，倒也无所谓。这场主要涉及暴力，二分查找等。
<!--more-->

# A. Maximum Increase
time limit per test1 second
memory limit per test256 megabytes
inputstandard input
outputstandard output

### Description
You are given array consisting of n integers. Your task is to find the maximum length of an increasing subarray of the given array.

A subarray is the sequence of consecutive elements of the array. Subarray is called increasing if each element of this subarray strictly greater than previous.

### Input
The first line contains single positive integer n (1 ≤ n ≤ 10^5) — the number of integers.
The second line contains n positive integers a1, a2, ..., an (1 ≤ ai ≤ 10^9).

### Output
Print the maximum length of an increasing subarray of the given array.

### Examples
input
`5
1 7 2 11 15`
output
`3`
input
`6
100 100 100 100 100 100`
output
`1`
input
`3
1 2 3`
output
`3`

这题没啥好说的意思是找最长严格上升子串，一个O(N)的扫描记录就可以了。

```cpp
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <iostream>
#include <algorithm>
#include <queue>
#include <set>
#include <vector>
#include <utility>
#include <map>
#define maxn 100005
#define INF 1000000005
#define mod 1000000007
using namespace std;
int a[maxn];
int main()
{
    int n,m,ans=0,cot=0;
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
    {
        scanf("%d",&a[i]);
    }
    for(int i=1;i<=n;i++)
    {
        if(a[i]>a[i-1])
            cot++;
        else
        {
            ans=max(ans,cot);
            cot=1;
        }
    }
    printf("%d\n",max(ans,cot));
    return 0;
}
```

***

# B. Powers of Two
time limit per test3 seconds
memory limit per test256 megabytes
inputstandard input
outputstandard output

### Description
You are given n integers a1, a2, ..., an. Find the number of pairs of indexes i, j (i < j) that ai + aj is a power of 2 (i. e. some integer x exists so that ai + aj = 2^x).

### Input
The first line contains the single positive integer n (1 ≤ n ≤ 10^5) — the number of integers.
The second line contains n positive integers a1, a2, ..., an (1 ≤ ai ≤ 10^9).

### Output
Print the number of pairs of indexes i, j (i < j) that ai + aj is a power of 2.

### Examples
input
`4
7 3 2 1`
output
`2`
input
`3
1 1 1`
output
`3`

### Note
In the first example the following pairs of indexes include in answer: (1, 4) and (2, 4).
In the second example all pairs of indexes (i, j) (where i < j) include in answer.

这题的意思求有多少对形如ai+aj的和等于2的次方。因为ai≤10^9,故也就二十几个2的次方个数。对于每个次方2^x来说枚举大于等于2^x/x=2^(x-1)的值才有意义，因为ai和aj里面必定至少要有一个大于等于2^(x-1)和才能凑够2^x。刚开始我还觉得会T，后来想想对于每个数y二分查找2^x-y之前出现的次数才O(nlogn)，绰绰有余。

```cpp
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <iostream>
#include <algorithm>
#include <queue>
#include <set>
#include <vector>
#include <utility>
#include <map>
#define maxn 100005
#define INF 2000000000
#define ll long long
using namespace std;
ll a[maxn],num[maxn];
int k=0;
void find_power()
{
    ll tmp=2;
    while(tmp<=INF)
    {
        num[k++]=tmp;
        tmp*=2;
    }
    return ;
}
int main()
{
    find_power();
    int n,p=0;
    ll ans=0;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        scanf("%I64d",&a[i]);
    }
    sort(a,a+n);
    for(int i=1;i<n;i++)
    {
        while(a[i]>=num[p]) p++;
        ll tmp=num[p]-a[i];
        ll *check=lower_bound(a,a+i,tmp);
        if(*check==tmp)
            ans+=upper_bound(a,a+i,tmp)-check;
    }
    printf("%I64d\n",ans);
    return 0;
}
```

***

# C. Cellular Network
time limit per test3 seconds
memory limit per test256 megabytes
inputstandard input
outputstandard output

### Description
You are given n points on the straight line — the positions (x-coordinates) of the cities and m points on the same line — the positions (x-coordinates) of the cellular towers. All towers work in the same way — they provide cellular network for all cities, which are located at the distance which is no more than r from this tower.

Your task is to find minimal r that each city has been provided by cellular network, i.e. for each city there is at least one cellular tower at the distance which is no more than r.

If r = 0 then a tower provides cellular network only for the point where it is located. One tower can provide cellular network for any number of cities, but all these cities must be at the distance which is no more than r from this tower.

### Input
The first line contains two positive integers n and m (1 ≤ n, m ≤ 10^5) — the number of cities and the number of cellular towers.

The second line contains a sequence of n integers a1, a2, ..., an ( - 10^9 ≤ ai ≤ 10^9) — the coordinates of cities. It is allowed that there are any number of cities in the same point. All coordinates ai are given in non-decreasing order.

The third line contains a sequence of m integers b1, b2, ..., bm ( - 10^9 ≤ bj ≤ 10^9) — the coordinates of cellular towers. It is allowed that there are any number of towers in the same point. All coordinates bj are given in non-decreasing order.

### Output
Print minimal r so that each city will be covered by cellular network.

### Examples
input
`3 2
-2 2 4
-3 0`
output
`4`
input
`5 3
1 5 10 14 17
4 11 15`
output
`3`

这题意思是说在一条直线上有一些城市，有一些信号塔，要求信号塔半径的范围最小，使得所有城市被覆盖。要倒过来想，对于每座城市只有两个选择，Rl和Rr,那肯定是取min(Rl,Rr)来的最优。所以ans=max(Σmin(Rl,Rr))。要注意边缘情况，在边缘的时候只需要考虑一遍的情况，我是额外加了两个信号塔，让他们坐标为INF和-INF。(这里还wa了一次)

```cpp
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <iostream>
#include <algorithm>
#include <queue>
#include <set>
#include <vector>
#include <utility>
#include <map>
#define maxn 100005
#define INF 2000000000
#define ll long long
using namespace std;
ll a[maxn],b[maxn];
int k=0;
int main()
{
    int n,m;
    scanf("%d%d",&n,&m);
    for(int i=0;i<n;i++)
    {
        scanf("%I64d",&a[i]);
    }
    for(int i=1;i<=m;i++)
    {
        scanf("%I64d",&b[i]);
    }
    b[0]=-INF;b[m+1]=INF;
    ll ans=0;
    for(int i=0;i<n;i++)
    {
        ll * tmp=upper_bound(b,b+m+1,a[i]);
        if((tmp-b)==m+1)
        {
            ans=max(ans,abs(a[i]-*(tmp-1)));
        }
        else if((tmp-b)==1)
        {
            ans=max(ans,abs(a[i]-*tmp));
        }
        else
        {
            ans=max(ans,min(abs(a[i]-*tmp),abs(a[i]-*(tmp-1))));
        }
    }
    printf("%I64d\n",ans);
    return 0;
}
```
***

待续...