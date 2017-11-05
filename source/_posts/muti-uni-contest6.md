---
title: 2016多校联合训练第6场
date: 2016-08-02 18:50:05
tags:
---
这场其实应该是数学场，但是硬是变成了打表找规律场。。
<!--more-->

# A Simple Nim

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 2522    Accepted Submission(s): 326


### Problem Description
Two players take turns picking candies from n heaps,the player who picks the last one will win the game.On each turn they can pick any number of candies which come from the same heap(picking no candy is not allowed).To make the game more interesting,players can separate one heap into three smaller heaps(no empty heaps)instead of the picking operation.Please find out which player will win the game if each of them never make mistakes.
 

### Input
Intput contains multiple test cases. The first line is an integer 1≤T≤100, the number of test cases. Each case begins with an integer n, indicating the number of the heaps, the next line contains N integers s[0],s[1],....,s[n−1], representing heaps with s[0],s[1],...,s[n−1] objects respectively.(1≤n≤106,1≤s[i]≤109)
 

### Output
For each test case,output a line whick contains either"First player wins."or"Second player wins".
 

### Sample Input
`2
2
4 4
3
1 2 4`
 

### Sample Output
`Second player wins.
First player wins.`

这题的意思其实很简单，就是一个组合游戏。n个石子堆，要么从某个石子堆里面取任意非空个石子，要么将一个石子堆分成三个非空石子堆。考虑其后继局面，可以暴力遍历后继局面打表。这里有一个坑是不能默认其中一种取法(类似nim博弈)的SG函数是从0-n-1，因为这种类似dp的打表，前面的值发生变化会影响后面的SG值。所以只能把其局面都暴力打出来，然后发现规律。打出的表如下图所示。
![](http://oava57hou.bkt.clouddn.com/multi-uni-contest6.jpg)

可以很清楚的看出只有 x%8==0时，sg[i]=i-1, 当x%8==7 sg[i]=i+1。剩下情况sg[i]=i。
```cpp
#include<cstdio>
#define maxn 1000005
using namespace std;
int a[maxn];
int main()
{
    int t;
    scanf("%d",&t);
    while(t--){
        int n;
        scanf("%d",&n);
        int ans=0;
        for(int i=0;i<n;i++){
            scanf("%d",&a[i]);
            if(a[i]%8==7)
            	ans^=(a[i]+1);
            else if(a[i]%8==0)
            	ans^=(a[i]-1);
            else 
            	ans^=a[i];
        }
        if(ans==0)
        	printf("Second player wins.\n");
        else
        	printf("First player wins.\n");
    }
    return 0;
}
```

***

# A Boring Question

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 897    Accepted Submission(s): 415


### Problem Description
![](http://oava57hou.bkt.clouddn.com/multi-uni-contest6_2.png)

### Input
The first line of the input contains the only integer ,
Then  lines follow,the i-th line contains two integers ,,
 

### Output
For each  and ,output the answer in a single line.
 

### Sample Input
`2
1 2
2 3`
 

### Sample Output
`3
13`

这题意思是给定一个n和m，有m位的一串数列，每个位置上可以从0-n任意取，也就是有(n+1)^m种取法，对于每一种取法求从组合乘积，再加起来求和。有一个简单的排除是，这个数列要是一个非降的序列。刚开始看无从下手，一个非常麻烦的推理过程，最后实在没法了，就开始暴力打表规律。因为这个组合数非常庞大，所以只打了n从1-9的表，用一个k代表这个组合数的每一位(因为正好是每一位只有一个数字),然后check函数里面要去掉 1.没有非降性的数列 2.数字的每一位不得遍历大于最大值的每一位(比如n取8，m取4，则最大值为8888，于是做一个从0-8888的循环，但是要每一位都小于等于8，而且有非降性。)这样我们就把所有符合条件的数列找了出来，然后对于每个数列进行组合乘积运算再求和。这么小的规模都大概跑了0.5s左右吧，可见暴力的数据量有多大。得到如图所示的下表
![](http://oava57hou.bkt.clouddn.com/multi-uni-contest6_3.jpg)

从表中可以看出一个(1,2)(2,2)(3,2)都是2*ans+1，(1,3)(2,3)(3,3)都是3*ans+1，于是就能总结出一个递推式子，对于(m,n)来说 `while(n--)ans=m*ans+1;`。因为m，n规模过大，所以只能用矩阵快速幂加速。注意在乘法的时候会爆int，所以要用ll来处理。

```cpp
#include <stdio.h>
#define MAX 2
#define ll long long
using namespace std;
typedef  struct{
        ll  m[MAX][MAX];
}  Matrix;//矩阵定义
Matrix I = {1,0,
            0,1,
           };
ll k=1000000007;
Matrix matrixmul(Matrix a,Matrix b)//矩阵乘法
{
       int i,j,h;
       Matrix c;
       for (i = 0 ; i < MAX; i++)
           for (j = 0; j < MAX;j++)
             {
                 c.m[i][j] = 0;
                 for (h = 0; h < MAX; h++)
                     c.m[i][j] += (a.m[i][h] * b.m[h][j])%k;
                 c.m[i][j] %=k;
             }
       return c;
}
Matrix quickpow(int n,Matrix P)//矩阵快速幂,对于一个矩阵A的n次方模k
{
       Matrix m = P, b = I;
       while (n >= 1)
       {
             if (n & 1)
                b = matrixmul(m,b);
             n = n >> 1;
             m = matrixmul(m,m);
       }
       return b;
}
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        ll ans=1,n,m;
        scanf("%I64d%I64d",&n,&m);
        Matrix P={m,1,0,1};
        I.m[0][0]=1;I.m[0][1]=0;I.m[1][0]=1;I.m[1][1]=0;
        Matrix last=quickpow(n,P);
        printf("%I64d\n",last.m[0][0]);
    }
    return 0;
}
```

这场就坑出了两题，好像还有一题是Lucas+容斥的简单题，下次有空补吧。。

