---
title: BestCoder Round85 题解
date: 2016-08-1 18:35:18
tags:
---
这场不知道啥时候开始的，等发现的时候快结束了。赛后的题，主要就是模拟，考的是思路。
<!--more-->

# Price List  
Accepts: 880   Submissions: 2184
 Time Limit: 2000/1000 MS (Java/Others)   Memory Limit: 262144/131072 K (Java/Others)

### 问题描述
在Byteland一共有n家商店，编号依次为1到n。每家商店只会卖一种物品，其中第i家商店的物品单价为vi。
Byteasar每天都会进行一次购物，他会在每家商店购买最多一件物品，当然他也可以选择什么都不买。回家之后，Byteasar会把这一天购物所花的钱的总数记录在账本上。
Byteasar的数学不好，他可能会把花的钱记少，也可能记多。Byteasar并不介意记少，因为这样看上去显得自己没花很多钱。
请写一个程序，帮助Byteasar判断每条记录是否一定记多了。

### 输入描述
输入的第一行包含一个正整数T(1≤T≤10)，表示测试数据的组数。

对于每组数据，第一行包含两个正整数n,m(1≤n,m≤100000)，表示商店的个数和记录的个数。

第二行包含nn个正整数vi(1≤vi≤100000)，依次表示每家商店的物品的单价。

接下来m行，每行包含一个整数q(0≤q≤10^18)，表示一条记录。

### 输出描述
对于每组数据，输出一行m个字符，依次回答每个询问。如果一定记多了，请输出'1'，否则输出'0'。

### 输入样例
`1
3 3
2 5 4
1
7
10000`

### 输出样例
`001`

这题就很简单了，就是判断qi>?sum{vi}。

```cpp
#include<stdio.h>
#include<iostream>
#include<math.h>
#include<algorithm>
#include<string>
#include<string.h>
#define maxn 100005
#define ll long long
using namespace std;
int main()
{
    int t;
    scanf("%d",&t);
    while(t--){
        int n,m;
        ll sum=0,tmp;
        scanf("%d%d",&n,&m);
        for(int i=0;i<n;i++)
        {
            scanf("%I64d",&tmp);
            sum+=tmp;
        }
        string ans;
        ll q;
        for(int i=0;i<m;i++)
        {
            scanf("%I64d",&q);
            if(q>sum)
                ans+="1";
            else
                ans+="0";
        }
        cout<<ans<<endl;
    }
    return 0;
}

```

***

# NanoApe Loves Sequence  
Accepts: 531   Submissions: 2481
 Time Limit: 2000/1000 MS (Java/Others)   Memory Limit: 262144/131072 K (Java/Others)

### 问题描述
退役狗 NanoApe 滚回去学文化课啦！
在数学课上，NanoApe 心痒痒又玩起了数列。他在纸上随便写了一个长度为 n的数列，他又根据心情随便删了一个数，这样他得到了一个新的数列，然后他计算出了所有相邻两数的差的绝对值的最大值。
他当然知道这个最大值会随着他删了的数改变而改变，所以他想知道假如全部数被删除的概率是相等的话，差的绝对值的最大值的期望是多少。

### 输入描述
第一行为一个正整数 T，表示数据组数。
每组数据的第一行为一个整数 n。

第二行为 n 个整数 Ai，表示这个数列。(1≤T≤10, 3≤n≤100000, 1≤Ai≤10^9)
​​ 
### 输出描述
对于每组数据输出一行一个数表示答案。
为防止精度误差，你需要输出答案乘上 n 后的值。

### 输入样例
`1
4
1 2 3 4`
### 输出样例
`6`

这题我刚开始还想用线段树或者优先队列模拟，后来想到，优先队列不就等价于数组排序后的结果。所以后来就直接数组排序，然后遍历每一个数，对于每一个数ai，检测abs(ai-a(i-1))和abs(ai-a(i+1))和排序后数组的第一大第二大的数字比较，若相同则先要去掉。(因为删除这个数后，等于是删除了这两个abs的值，再加上后一个abs的值)然后结再和abs(a(i+1)-a(i-1))的结果取一个最大值。因为最后结果要求乘n，所以这个就直接是答案输出，注意爆int用ll。

```cpp
#include<stdio.h>
#include<math.h>
#include<algorithm>
#define maxn 100005
#define ll long long
using namespace std;
ll a[maxn],d[maxn];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--){
        int n;
        ll ans=0;
        scanf("%d",&n);
        for(int i=0;i<n;i++)
        {
            scanf("%I64d",&a[i]);
            if(i!=0)
                d[i-1]=abs(a[i]-a[i-1]);
        }
        sort(d,d+n-1);
        for(int i=0;i<n;i++)
        {
            if(i==0)
            {
                if(d[n-2]==abs(a[0]-a[1]))
                    ans+=d[n-3];
                else
                    ans+=d[n-2];
            }
            else if(i==n-1)
            {
                if(d[n-2]==abs(a[n-1]-a[n-2]))
                    ans+=d[n-3];
                else
                    ans+=d[n-2];
            }
            else
            {
                ll tmp1=max(abs(a[i]-a[i-1]),abs(a[i]-a[i+1]));
                ll tmp2=min(abs(a[i]-a[i-1]),abs(a[i]-a[i+1]));
                ll tmp3=abs(a[i-1]-a[i+1]);
                if(d[n-2]==tmp1)
                {
                    if(d[n-3]==tmp2)
                    {
                        ans+=max(d[n-4],tmp3);
                    }
                    else
                        ans+=max(d[n-3],tmp3);
                }
                else
                    ans+=max(d[n-2],tmp3);
            }
        }
        printf("%I64d\n",ans);
    }
    return 0;
}
```

***

# NanoApe Loves Sequence Ⅱ  
Accepts: 374   Submissions: 946
 Time Limit: 4000/2000 MS (Java/Others)   Memory Limit: 262144/131072 K (Java/Others)

### 问题描述
退役狗 NanoApe 滚回去学文化课啦！

在数学课上，NanoApe 心痒痒又玩起了数列。他在纸上随便写了一个长度为 n 的数列，他又根据心情写下了一个数 m。

他想知道这个数列中有多少个区间里的第 k 大的数不小于 m，当然首先这个区间必须至少要有 k 个数啦。

### 输入描述
第一行为一个正整数 T，表示数据组数。

每组数据的第一行为三个整数 n,m,k。

第二行为 n 个整数 Ai，表示这个数列。(1≤T≤10, 2≤n≤200000, 1≤k≤n/2, 1≤m, Ai≤10^9)
​​ 
### 输出描述
对于每组数据输出一行一个数表示答案。

### 输入样例
`1
7 4 2
4 2 7 7 6 5 1`
### 输出样例
`18`

这题其实就是用取尺法的思路，设定左指针从左往右遍历ai，然后用一个右指针p扩张区间，区间里面包含k个大于等于m的数的话，就是符合条件的，记录下包含该区间的所有区间，然后继续扩张。需要注意的是遍历过的左指针位置如果是大于等于m的话需要去掉一个sum，需要用ll存ans。

```cpp
#include<stdio.h>
#include<iostream>
#include<math.h>
#include<algorithm>
#include<string>
#include<string.h>
#define maxn 200005
#define ll long long
using namespace std;
int a[maxn];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--){
        int n,m,k;
        scanf("%d%d%d",&n,&m,&k);
        for(int i=0;i<n;i++)
        {
            scanf("%d",&a[i]);
            if(a[i]>=m)
                a[i]=1;
            else
                a[i]=0;
        }
        int p=0,sum=0;
        ll ans=0;
        for(int i=0;i<n;i++)
        {
            while((sum<k)&&(p<n))
            {
                sum+=a[p++];
            }
            if(sum>=k)
                ans+=(ll)(n-p+1);
            if(a[i]==1)
                sum--;
        }
        printf("%I64d\n",ans);
    }
    return 0;
}

```
