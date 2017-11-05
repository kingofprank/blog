---
title: 2016多校联合训练第5场
date: 2016-08-02 18:50:05
tags:
---
这一场有贪心，容斥，树状数组和dp，感觉dp还是很差劲。
<!--more-->

# Divide the Sequence

Time Limit: 5000/2500 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
Alice has a sequence A, She wants to split A into as much as possible continuous subsequences, satisfying that for each subsequence, every its prefix sum is not small than 0.
 

### Input
The input consists of multiple test cases. 
Each test case begin with an integer n in a single line.
The next line contains n integers A1,A2...An.
1≤n≤1e6
−10000≤A[i]≤10000
You can assume that there is at least one solution.
 

### Output
For each test case, output an integer indicates the maximum number of sequence division.
 

### Sample Input
`6
1 2 3 4 5 6
4
1 2 -3 0
5
0 0 0 0 0`
 

### Sample Output
`6
2
5`

这题意思是尽量分割数组A成为尽可能多的段，使得每一段的所有前缀和都大于等于0。先做整体前缀和，单独考虑对于一个负数X来说，只有当出现第一个sum[y]<=sum[x](y<x)的时候，sum[i]-sum[y]>=0(y<=i<=x)，于是该段的所有前缀和大于等于0，也是满足条件的最小段。于是从后往前贪心就行了，遇到一个负数就开始向前找满足条件的y。比赛的时候wa了3次，坑爹的是因为贪心的时候i再次赋值出现了问题，第一次maxn多写了一个0，开大了(这个错误太坑爹了)，第二次赋值i=tmp-1.第三次i=tmp，然后正确的赋值是i=tmp+1。

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
ll a[maxn],sum[maxn];
int main()
{
    int n;
    while(~scanf("%d",&n))
    {
        int tmp,ans=0;
        for(int i=1;i<=n;i++)
        {
            scanf("%I64d",&a[i]);
            sum[i]=sum[i-1]+a[i];
        }
        for(int i=n;i>=1;i--)
        {
            if(a[i]>=0)
                ans++;
            else
            {
                int tmp=i-1;
                while(sum[tmp]>sum[i])
                    tmp--;
                i=tmp+1;
                ans++;
            }
        }
        printf("%d\n",ans);
        memset(a,0,sizeof(a));
        memset(sum,0,sizeof(sum));
    }
    return 0;
}
```

***

# Two

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
Alice gets two sequences A and B. A easy problem comes. How many pair of sequence A' and sequence B' are same. For example, {1,2} and {1,2} are same. {1,2,4} and {1,4,2} are not same. A' is a subsequence of A. B' is a subsequence of B. The subsequnce can be not continuous. For example, {1,1,2} has 7 subsequences {1},{1},{2},{1,1},{1,2},{1,2},{1,1,2}. The answer can be very large. Output the answer mod 1000000007.


### Input
The input contains multiple test cases.

For each test case, the first line cantains two integers N,M(1≤N,M≤1000). The next line contains N integers. The next line followed M integers. All integers are between 1 and 1000.


### Output
For each test case, output the answer mod 1000000007.


### Sample Input
`3 2
1 2 3
2 1
3 2
1 2 3
1 2`


### Sample Output
`2
3`

这道题的意思是给定A，B两个数组，问有多少子序列的组合相等，答案输出取模。讲道理这题一眼看过去就是dp，但是还是没写出来，感觉dp都很菜鸡。dp[i][j]代表A数组前i位和B数组前j位有多少种组合的个数(可取也可以不取这一位)。当a[i]=b[j]的时候，显然有dp[i][j]=dp[i-1][j-1](不取第i，j位)+dp[i-1][j-1](取第i，j位和前面组合)+1(单独第i，j位)。当a[i]!=b[j]的时候有，dp[i][j]=dp[i-1][j]+dp[i][j-1](取第i位和B数组组合，第j位和A数组组合，因为不可能同时取第i，j位，同时不取第i，j位的时候已经包括在这两个dp中，而且还有重复)-dp[i-1][j-1](减去同时不包含第i，j位的重复项)，于是状态转移方程就写好了。下面就是初始化dp[i][1]和dp[1][j]，当a[i]=b[1]或者a[1]=b[j]的时候，就加上1，累加递增。需要注意的是，因为答案有可能出现负数(因为有减法后取模)，所以输出的时候要(ans+mod)%mod!!这题由黄神强力A掉，下面附上黄神的代码。

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
const int maxn=1111;
const int mod=1e9+7;
typedef long long ll;
int a[maxn],b[maxn];
ll dp[maxn][maxn];
int main()
{
    int n,m;
    while(~scanf("%d%d",&n,&m)){
        memset(dp,0,sizeof(dp));
        memset(a,0,sizeof(a));
        memset(b,0,sizeof(b));
        for(int i=1;i<=n;i++)scanf("%d",&a[i]);
        for(int i=1;i<=m;i++)scanf("%d",&b[i]);
        for(int i=1;i<=n;i++)dp[1][i]=dp[1][i-1]+(a[i]==b[1]?1:0);
        for(int i=1;i<=m;i++)dp[i][1]=dp[i-1][1]+(a[1]==b[i]?1:0);
        for(int i=2;i<=m;i++){
            for(int j=2;j<=n;j++){
                if(b[i]==a[j])
                    dp[i][j]=(dp[i-1][j]+dp[i][j-1]+1)%mod;
                else 
                    dp[i][j]=(dp[i-1][j]+dp[i][j-1]-dp[i-1][j-1])%mod;
            }
        }
        printf("%lld\n",(dp[m][n]+mod)%mod);
    }
}
```

***

# World is Exploding

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
Given a sequence A with length n,count how many quadruple (a,b,c,d) satisfies: a≠b≠c≠d,1≤a<b≤n,1≤c<d≤n,Aa<Ab,Ac>Ad.
 

### Input
The input consists of multiple test cases. 
Each test case begin with an integer n in a single line.

The next line contains n integers A1,A2...An.
1≤n≤50000
0≤Ai≤1e9
 

### Output
For each test case,output a line contains an integer.
 

### Sample Input
`4
2 4 1 3
4
1 2 3 4`
 

### Sample Output
`1
0`

这题的意思是找一个序号不同的四元组，使得有一组递增，一组递减的关系，问在给定的序列里面有多少个这样的四元组。这题刚开始看过去觉得很麻烦，因为不能重复，而且n也很大，不能两重枚举，不然会超时。所以只能用O(NlogN)的算法，考虑到两组的单调性。故可以用容斥原理，先是算出所有的递增组up和所以的递减组down，所有的组合是up*down，然后减去重复的四种情况。设L/Ru[x]分别为第x位左/右侧大于该数的个数，同理L/Rd[x]分别为第x位左/右侧小于该数的个数。每一位上重复的四种情况分别是Lu[i]*Ru[i]，Ld[i]*Rd[i]，Lu[i]*Ld[i]和Ru[i]*Rd[i]。所以用树状数组统计个数，因为数据太大，所以之前要先hash一下。

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include <map>
#include <set>
using namespace std;
typedef long long ll;
const int maxn=50010;
inline int lowbit(int x)
{
    return x&-x;
}
void add(int x,int num,int cnt[])//cnt[x]增加num
{
    while(x<maxn){
        cnt[x]+=num;
        x+=lowbit(x);
    }
}
int sum(int x,int cnt[])//求1-x的区间和
{
    int sum=0;
    while(x>0){
        sum+=cnt[x];
        x-=lowbit(x);
    }
    return sum;
}
ll Ld[maxn],Lu[maxn],Rd[maxn],Ru[maxn],pre[maxn];
int cnt[maxn],a[maxn],num[maxn];
int main()
{
    int n;
    while(~scanf("%d",&n)){
        map<int, set<int> >G;
        for(int i=1;i<=n;i++)
        {
            scanf("%d",&a[i]);
            G[a[i]].insert(i);
        }
        int cot=0;
        for(map< int , set<int> >::iterator iter=G.begin();iter!=G.end();iter++)
        {
            cot++;
            for(set<int>::iterator jter=(iter->second).begin();jter!=(iter->second).end();jter++)
            {
                num[(*jter)]=cot;
            }
        }
        ll up=0,down=0;
        for(int i=1;i<=n;i++)
        {
            add(num[i],1,cnt);
            pre[num[i]]++;
            Ld[i]=sum(num[i]-1,cnt);
            up+=Ld[i];
            Lu[i]=i-pre[num[i]]-Ld[i];
            down+=Lu[i];
        }
        memset(cnt,0,sizeof(cnt));
        memset(pre,0,sizeof(pre));
        for(int i=n;i>=1;i--)
        {
            add(num[i],1,cnt);
            pre[num[i]]++;
            Rd[i]=sum(num[i]-1,cnt);
            Ru[i]=(n-i+1)-pre[num[i]]-Rd[i];
        }
        ll ans=down*up;
        for(int i=1;i<=n;i++)
        {
            ans=ans-Lu[i]*Ru[i]-Ld[i]*Rd[i]-Lu[i]*Ld[i]-Ru[i]*Rd[i];
        }
        printf("%I64d\n",ans);
        memset(Lu,0,sizeof(Lu));
        memset(Ru,0,sizeof(Ru));
        memset(Ld,0,sizeof(Ld));
        memset(Rd,0,sizeof(Rd));
        memset(cnt,0,sizeof(cnt));
        memset(a,0,sizeof(a));
        memset(num,0,sizeof(num));
        memset(pre,0,sizeof(pre));
    }
    return 0;
}
```

这场比赛后面的时间坑在一道概率dp上，其实是求期望的dp，但是不熟练，想了很久，还是没写掉。

