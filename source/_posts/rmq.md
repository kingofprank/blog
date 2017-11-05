---
title: RMQ和ST算法
date: 2015-08-11 20:43:00
tags: [数据结构]
---
简要说明一下RMQ问题所用的ST算法
<!--more-->
# 初识RMQ
RMQ问题是查询区间最值问题，用于给定一个序列A，以RMQ(A,i,j)这种方式询问。

下面介绍ST算法(sparse-table),主要思想就是动态规划,以O(logn)的时间预处理,n次询问用O(1)的时间回答,时间复杂度为O(nlogn)

F[i,j]表示为从第i个数开始(包含自己),连续2^j个数中的最大值或者最小值。也就是从**[i,i+2^j-1]**区间。

很显然F[i,0]就等于A[i]本身,这便是初值。状态转移方程,把F[i,j]分成两段**[i,i+2^(j-1)-1]**和**[i+2^(j-1),i+2^j-1]**,每个区间长都为**2^(j-1)**。

于是动态规划的方程**F[i,j]=max/min(F[i,j-1],F[i+2^(j-1),j-1])**,下面进行DP就可以了，以下是伪代码。
```cpp
for i : 1 to n
	F[i][0]=A[i]//初始化
for j : 1 to log(n)/log(2)
	for i : 1 to (n-1+2^i)
		F[i][j]=max/min(F[i][j-1],F[i+2^(j-1)][j-1])
```
下面便是查询RMQ(i,j)
```cpp
k=log(j-i+1)/log2
return max/min(F[i][k],F[j-2^k+1][k])
```
因为计算k用log时间会花的稍微长一些可以用来优化
```cpp
k=0;x=2;range=j-i+1;
while(x<=range){k++;x<<=1;}
```

下面给出模板,b可以当做全局变量
```cpp
int dp[100][100];
void markrmq(int n,int b[])//n是长度,b是要求的数组,从b[0]开始放置,达不到n
{
    int i,j;
    for(i=0;i<n;i++)
        dp[i][0]=b[i];
    for(j=1;(1<<j)<=n;j++)
        for(i=0;i+(1<<j)-1<n;i++)
            dp[i][j]=min(dp[i][j-1],dp[i+(1<<(j-1))][j-1]);
}
int rmq(int i,int j)
{
    int k=(int)(log((j-i+1)*1.0)/log(2.0));
    return min(dp[i][k],dp[j-(1<<k)+1][k]);
}
int main()
{
    int i,j;
    int b[10]={10,1,45,23,14,5,68,5,9,7};
    markrmq(10,b);
    while(~scanf("%d%d",&i,&j))
    {
        cout<<rmq(i,j)<<endl;
    }
    return 0;
}
```

返回最小值的下标,其实和RMQ一模一样,只是把dp里面存的是最小值改成最小值下标了,同理b[]可以作为全局变量。
```cpp
void makeRmqIndex(int n,int b[]) //返回最小值对应的下标
{
    int i,j;
    for(i=0;i<n;i++)
        dp[i][0]=i;
    for(j=1;(1<<j)<=n;j++)
        for(i=0;i+(1<<j)-1<n;i++)
            dp[i][j]=b[dp[i][j-1]] < b[dp[i+(1<<(j-1))][j-1]]? dp[i][j-1]:dp[i+(1<<(j-1))][j-1];//RMQ比较的是两个dp的值,这里比较的是两个区间最小值返回更小的那个的下标
}
int rmqIndex(int s,int v,int b[])
{
    int k=(int)(log((v-s+1)*1.0)/log(2.0));
    return b[dp[s][k]]<b[dp[v-(1<<k)+1][k]]? dp[s][k]:dp[v-(1<<k)+1][k];//返回下标
}
```