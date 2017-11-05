---
title: 2016多校联合训练第4场
date: 2016-07-28 22:30:39
tags: 
---
这一场有挺多的字符串处理的题目，字符串题目是弱项啊TAT。所以就GG了。涉及内容有的逆序对，暴力和kmp。dp等。
<!--more-->

# 1012 Bubble Sort

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
P is a permutation of the integers from 1 to N(index starting from 1).
Here is the code of Bubble Sort in C++.

for(int i=1;i<=N;++i)
    for(int j=N,t;j>i;—j)
        if(P[j-1] > P[j])
            t=P[j],P[j]=P[j-1],P[j-1]=t;

After the sort, the array is in increasing order. ?? wants to know the absolute values of difference of rightmost place and leftmost place for every number it reached.
 

### Input
The first line of the input gives the number of test cases T; T test cases follow.
Each consists of one line with one integer N, followed by another line with a permutation of the integers from 1 to N, inclusive.

limits
T <= 20
1 <= N <= 100000
N is larger than 10000 in only one case. 
 

### Output
For each test case output “Case #x: y1 y2 … yN” (without quotes), where x is the test case number (starting from 1), and yi is the difference of rightmost place and leftmost place of number i.
 

### Sample Input
`2
3
3 1 2
3
1 2 3`
 

### Sample Output
`Case #1: 1 1 2
Case #2: 0 0 0`

### Hint
In first case, (3, 1, 2) -> (3, 1, 2) -> (1, 3, 2) -> (1, 2, 3)
the leftmost place and rightmost place of 1 is 1 and 2, 2 is 2 and 3, 3 is 1 and 3
In second case, the array has already in increasing order. So the answer of every number is 0.

题意是给定一个从后往前的冒泡排序，每次都将最小的值放到原位。问对于每个值而言，它所处的最左边和最右边的差的绝对值是多少。刚开始看的时候就觉得是逆序对，分析了一下对于每个数来说，能向右边移动的步数其实就是在它后面比它来的小的个数，也就是逆序数。最左边的位置就是原位和当前位的最小值。所以一个简单的树状数组加上判断就可以了。

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
#define INF 1000000005
using namespace std;
const int maxn=100005;
int cnt[maxn],ans[maxn],a[maxn];
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
int main()
{
    int t,num=1;
    scanf("%d",&t);
    while(t--)
    {
        int n;
        scanf("%d",&n);
        for(int i=1;i<=n;i++)
            scanf("%d",&a[i]);
        for(int i=n;i>=1;i--)
        {
            int tmp=sum(a[i],cnt);
            ans[a[i]]=abs(max(a[i],max(i,i+tmp))-min(a[i],min(i,i+tmp)));
            add(a[i],1,cnt);
        }
        printf("Case #%d: ",num++);
        for(int i=1;i<n;i++)
        {
            printf("%d ",ans[i]);
        }
        printf("%d\n",ans[n]);
        memset(cnt,0,sizeof(cnt));
        memset(a,0,sizeof(a));
        memset(ans,0,sizeof(ans));
    }
    return 0;
}
```
***

# 1011 Where Amazing Happens

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
As the premier men's professional basketball league in the world, the National Basketball Association (NBA) has witnessed many superstars, legendary teams and precious friendships. Here is a list of every season’s champion from the league's inception in 1946 to 2015.

Season Champion
2015-16 Cleveland Cavaliers
2014-15 Golden State Warriors
2013-14 San Antonio Spurs
2012-13 Miami Heat
2011-12 Miami Heat
2010-11 Dallas Mavericks
...
1950-51 Rochester Royals
1949-50 Minneapolis Lakers
1948-49 Minneapolis Lakers
1947-48 Baltimore Bullets
1946-47 Philadelphia Warriors

Given the team name, it won’t be difficult for you to count how many times this team(with exactly the same name) has made amazing happen.

### Input
The first line gives the number of test cases. Each case contains one string S representing the team to be queried.
T<=30.S consists of English letters, digits, punctuations and spaces. And 1<=length(S)<=30.
 

### Output
For each test case, output one line containing "Case #x: y", where x is the test case number (starting from 1) and y is the times this team has won the championship according to the list above.

### Sample Input
`2
Cleveland Cavaliers
Oklahoma City Thunder`
 

### Sample Output
`Case #1: 1
Case #2: 0`

这题就是水题，给定一个NBA队伍名，查询在表中出现多少次。用一个map套一下就行了。贴上成神的代码

```cpp
#include <stdio.h>
#include <string>
#include <map>

int main(int argc, char const *argv[]){
	static std::string T[70] = {"Cleveland Cavaliers","Golden State Warriors","San Antonio Spurs","Miami Heat"..};
	std::map<std::string, int> M;
    for (int i = 0; i < 70; ++M[T[i]], ++i);
    char s[100];
    int  t, cnt=0;
    scanf("%d\n",&t);
    while(cnt<t) {
        ++cnt;
        gets(s);
        printf("Case #%d: %d\n", cnt, M[s]);
    }
    return 0;
}
```
***

# 1001 Another Meaning

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 65536/65536 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
As is known to all, in many cases, a word has two meanings. Such as “hehe”, which not only means “hehe”, but also means “excuse me”. 
Today, ?? is chating with MeiZi online, MeiZi sends a sentence A to ??. ?? is so smart that he knows the word B in the sentence has two meanings. He wants to know how many kinds of meanings MeiZi can express.
 

### Input
The first line of the input gives the number of test cases T; T test cases follow.
Each test case contains two strings A and B, A means the sentence MeiZi sends to ??, B means the word B which has two menaings. string only contains lowercase letters.

Limits
T <= 30
|A| <= 100000
|B| <= |A|

 

### Output
For each test case, output one line containing “Case #x: y” (without quotes) , where x is the test case number (starting from 1) and y is the number of the different meaning of this sentence may be. Since this number may be quite large, you should output the answer modulo 1000000007.
 

### Sample Input
`4
hehehe
hehe
woquxizaolehehe
woquxizaole
hehehehe
hehe
owoadiuhzgneninougur
iehiehieh`
 

### Sample Output
`Case #1: 3
Case #2: 2
Case #3: 5
Case #4: 1`

### Hint
In the first case, “ hehehe” can have 3 meaings: “*he”, “he*”, “hehehe”.
In the third case, “hehehehe” can have 5 meaings: “*hehe”, “he*he”, “hehe*”, “**”, “hehehehe”.

这题坑了我好久，可能面对字符串题目就感觉虚弱。题意是给定一个主串，再给一个模式串，模式串可以选择两种意思(有两种状态)，问在主串中可以翻译出来多少种不同意思。如果不出现模式串的话也会有一种意思。首先很显然要用kmp算出每个匹配的位置。这里拷了一发模板。然后接下来就是一个dp，这个dp其实挺简单的。dp[i]代表以i位置之前有多少种解法。如果i正好是一个匹配的结尾+1的话，**dp[i]=dp[i-1]+dp[i-lenp]**，代表i位置之前的长度为lenp的字符串选择或者不选择。初始化下最开始的点是1。我这里比较麻烦，因为是从0开始的，所以要特判i=0的时候直接赋值。

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
#define maxn 200005
#define INF 1000000005
#define mod 1000000007
using namespace std;
vector<int>find_substring(string pattern,string text)
{
    int n=pattern.size();
    vector<int>next(n+1,0);
    for(int i=1;i<n;i++)
    {
        int j=i;
        while(j>0)
        {
            j=next[j];
            if(pattern[j]==pattern[i])
            {
                next[i+1]=j+1;
                break;
            }
        }
    }
    vector<int>positions;
    int m=text.size();
    for(int i=0,j=0;i<m;i++)
    {
        if(j<n&&text[i]==pattern[j])
            j++;
        else
        {
            while(j>0)
            {
                j=next[j];
                if(text[i]==pattern[j])
                {
                    j++;
                    break;
                }
            }
        }
        if(j==n)
            positions.push_back(i-n+1);
    }
    return positions;
}
string pattern,text;
vector<int>G;
long long int dp[maxn];
bool used[maxn];
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    int num=1;
    while(t--)
    {
        cin>>text>>pattern;
        G=find_substring(pattern,text);
        int p=G.size();
        int lent=text.size(),lenp=pattern.size();
        for(int i=0;i<p;i++)
            used[G[i]+lenp]=true;
        if(G.size()==0) printf("Case #%d: 1\n",num++);
        else{
            for(int i=0;i<=lent;i++)
            {
                if(used[i]==true)
                {
                    if(i==0)
                        dp[i]=1;
                    else
                    {
                        if(i-lenp<0)
                            dp[i]=dp[i-1];
                        else
                            dp[i]=(dp[i-1]+dp[i-lenp])%mod;
                    }
                    p++;
                }
                else
                {
                    if(i==0)
                        dp[i]=1;
                    else
                        dp[i]=dp[i-1];
                }
            }
            printf("Case #%d: %I64d\n",num++,dp[lent]);
            memset(dp,0,sizeof(dp));
            memset(used,false,sizeof(used));
            G.clear();
        }
    }
    return 0;
}
```

这场就做了三题，有一题还没看，因为这场的时候出了点事故。手机差点报废了TAT。。
