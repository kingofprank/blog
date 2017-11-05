---
title: BestCoder Round85 题解
date: 2016-08-1 18:35:18
tags:
---
这场是赛后补的，题目涉及抽屉原理，素数筛等数学内容。
<!--more-->

# Sum  
Accepts: 640   Submissions: 1744 Time Limit: 2000/1000 MS (Java/Others)   Memory Limit: 131072/131072 K (Java/Others)

### 问题描述
给定一个数列，求是否存在连续子列和为m的倍数，存在输出YES，否则输出NO。

### 输入描述
输入文件的第一行有一个正整数T(1≤T≤10)，表示数据组数。
接下去有T组数据，每组数据的第一行有两个正整数n,m (1≤n≤100000 , 1≤m≤5000)。
第二行有n个正整数x (1≤x≤100)表示这个数列。

### 输出描述
输出T行，每行一个YES或NO。

### 输入样例
`2
3 3
1 2 3
5 7
6 6 6 6 6`

### 输出样例
`YES
NO`

这题类似多校训练3中的1011求哈密顿距离的题目，同样看似需要O(N^2)的遍历，其实由于%m的值只会有0~5000的范围，所以根据抽屉原理，一定能在O(m)的时间复杂度内解决一次询问。故直接暴力就行啦，记录一下前缀和sum[i]%m的值就行了，如果出现重复的值则两者之间的序列和必定是m的倍数，因为sum[i]%m==(sum[i]+m*k)%m。要注意初始化的时候check[0]=true！！！

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
int a[maxn],sum[maxn];
bool check[maxn];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n,m;
        bool flag=false;
        scanf("%d%d",&n,&m);
        check[0]=true;
        for(int i=1;i<=n;i++)
        {
            scanf("%d",&a[i]);
        }
        for(int i=1;i<=n;i++)
        {
            sum[i]=(sum[i-1]+a[i])%m;
            if(check[sum[i]%m]==false)
                check[sum[i]%m]=true;
            else
            {
                flag=true;
                break;
            }
        }
        if(flag==true)
            printf("YES\n");
        else
            printf("NO\n");
        memset(sum,0,sizeof(sum));
        memset(a,0,sizeof(a));
        memset(check,false,sizeof(check));
    }
    return 0;
}
```

***

# Domino  
Accepts: 462   Submissions: 1498
Time Limit: 2000/1000 MS (Java/Others)   Memory Limit: 131072/131072 K (Java/Others)

### 问题描述
小白在玩一个游戏。桌子上有n张多米诺骨牌排成一列。它有k次机会，每次可以选一个还没有倒的骨牌，向左或者向右推倒。每个骨
牌倒下的时候，若碰到了未倒下的骨牌，可以把它推倒。小白现在可以随意设置骨牌的高度，但是骨牌高度为整数，且至少为1，并且
小白希望在能够推倒所有骨牌的前提下，使所有骨牌高度的和最小。

### 输入描述
第一行输入一个整数T(1≤T≤10)
每组数据有两行
第一行有两个整数n和k，分别表示骨牌张数和机会次数。(2≤k,n≤100000)
第二行有n-1个整数，分别表示相邻骨牌的距离d,1≤d≤100000。

### 输出描述
对于每组数据，输出一行，最小的高度和

### 输入样例
`1
4 2
2 3 4`

### 输出样例
`9`

这题可以看作都往右边倒下，因为骨牌往左边倒和右边倒其实都是一样的。那么其实就是从大到小排序后，选择前min(n,k)大的数为起点向后推，在min(n,k)段中，段的最后一个骨牌高度为1，剩下骨牌高度均为相邻距离+1。

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
ll a[maxn];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int k,n;
        scanf("%d%d",&n,&k);
        for(int i=0;i<n-1;i++)
            scanf("%I64d",&a[i]);
        sort(a, a+n-1, greater<ll>() );
        int len=min(n,k);
        ll ans=0;
        for(int i=0;i<n-1;i++)
        {
            if(i<len-1)
                ans+=1;
            else
                ans+=a[i]+1;
        }
        printf("%I64d\n",ans+1);
        memset(a,0,sizeof(a));
    }
    return 0;
}
```

***

# Abs  
Accepts: 136   Submissions: 927
Time Limit: 2000/1000 MS (Java/Others)   Memory Limit: 131072/131072 K (Java/Others)

### 问题描述
给定一个数x，求正整数y(y≥2)，使得满足以下条件：
1.y-x的绝对值最小
2.y的质因数分解式中每个质因数均恰好出现2次。
输入描述
第一行输入一个整数T（1≤T≤50)
每组数据有一行，一个整数x（1≤x≤10^18)

### 输出描述
对于每组数据，输出一行y-x的最小绝对值

### 输入样例
`5
1112
4290
8716
9957
9095`

### 输出样例
`23
65
67
244
70`

这题可以很显然的想到对于y来说，必定是一个完全平方数，故只需要考虑z=sqrt(y)的值。z的质因子分解后每个素数只能出现一次，因为素数定理关系，比较稠密，故只需要暴力枚举z(检查z是否符合该条件)就可以算出答案。从sqrt(x)附近开始向上，向下枚举。枚举的时候要注意，不可以同时比较向上枚举第k个，向下枚举第k个。因为有可能出现向下第i个，和向上第j个(i<j)，但是算出来的abs却是第j个来的小的情况。(比如999999的时候)。故更改枚举的方法，分别向下和向下枚举到符合条件的一个数再加上自身，由三个条件来算出最小的abs。（这里坑了我一个小时查错TAT)

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
#define maxn 1000005
#define INF 1000000
#define ll long long
using namespace std;
ll prime[maxn];
int k=0;
bool used[maxn];
void find_prime()
{
    for(int i=2;i<=INF;i++)
    {
        if(used[i]==false)
        {
            used[i]=true;
            prime[k++]=i;
            int cot=2;
            while(cot*i<=INF)
            {
                used[cot*i]=true;
                cot++;
            }
        }
    }
}
ll ans=2e18,x;
bool check(ll tmp)
{
    if(tmp<2) return false;
    ll now=tmp;
    for(int i=0;i<k&&prime[i]*prime[i]<=now;i++)
    {
        if(tmp%prime[i]==0)
        {
            int cot=0;
            while(tmp%prime[i]==0)
            {
                cot++;
                tmp/=prime[i];
            }
            if(cot>1)
                return false;
        }
        if(tmp<prime[i])
            break;
    }
    ans=min(ans,abs(now*now-x));
    return true;
}
int main()
{
    find_prime();
    int t;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%I64d",&x);
        ll z=(ll)(sqrt(x*1.0));
        check(z);
        int i=1;
        while(!check(z+i)) i++;
        i=1;
        while(!check(z-i)) i--;
        printf("%I64d\n",ans);
        ans=2e18;
    }
    return 0;
}
```

剩下的两道就比较麻烦了，一道是dp，还有一道是数论。暂且先放着吧(虽然以后也不一定补了= =。)


