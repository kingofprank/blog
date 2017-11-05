---
title: 2016多校联合训练第3场
date: 2016-07-26 21:56:04
tags: 
---
这一场各种猜结论，有的简单题目出的比较慢。主要内容有简单模拟，排列组合，博弈和高中物理。
<!--more-->
# 1001 Sqrt Bo
Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 131072/131072 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0

### Problem Description
Let's define the function f(n)=[√n].

Bo wanted to know the minimum number y which satisfies fy(n)=1.

note:f1(n)=f(n),fy(n)=f(fy−1(n))

It is a pity that Bo can only use 1 unit of time to calculate this function each time.

And Bo is impatient, he cannot stand waiting for longer than 5 units of time.

So Bo wants to know if he can solve this problem in 5 units of time.

### Input
This problem has multi test cases(no more than 120).

Each test case contains a non-negative integer n(n<10100).

### Output
For each test case print a integer - the answer y or a string "TAT" - Bo can't solve this problem.

### Sample Input
`233
233333333333333333333333333333333333333333333333333333333`

### Sample Output
`3
TAT`

这道题目意思是有个f(n)函数，对n开根号向下取整。最多进行5次，问能不能在5次之后等于1。其实每一次的限制在1<=x<(Last_max)^2内就可以做到等于1，Last_max在第一次是2，所以分界线是4294967296。很简单的先判断一下长度有没有大于这个长度，然后写个函数恢复成长整型就可以了。贴上成神的代码。
```cpp
#include <stdio.h>
#include <string.h>
#include <math.h>

int main() {
    char s[200];
    while(~scanf("%s", s)){
        int l = strlen(s);
        if(l<12) {
            long long a = 0;
            char *p = s;
            while(p<s+l) {
                a = a*10 + *p-'0';
                ++p;
            }
            int cnt = 0;
            while(cnt<5&&a!=1) {
                double x=a;
                x = sqrt(x);
                a = (long long)x;
                ++cnt;
            }
            if(cnt<=5&&a==1) {
                printf("%d\n",cnt);
            } else {
                printf("TAT\n");
            }
        } else {
            printf("TAT\n");
        }
    }
}
```

***

# 1011 Teacher Bo

Time Limit: 4000/2000 MS (Java/Others)    Memory Limit: 131072/131072 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
Teacher BoBo is a geography teacher in the school.One day in his class,he marked N points in the map,the i-th point is at (Xi,Yi).He wonders,whether there is a tetrad (A,B,C,D)(A<B,C<D,A≠CorB≠D) such that the manhattan distance between A and B is equal to the manhattan distance between C and D.

If there exists such tetrad,print "YES",else print "NO".
 

### Input
First line, an integer T. There are T test cases.(T≤50)

In each test case,the first line contains two intergers, N, M, means the number of points and the range of the coordinates.(N,M≤105).

Next N lines, the i-th line shows the coordinate of the i-th point.(Xi,Yi)(0≤Xi,Yi≤M).
 

### Output
T lines, each line is "YES" or "NO".

### Sample Input
`2
3 10
1 1
2 2
3 3
4 10
8 8
2 3
3 3
4 4`
 

### Sample Output
`YES
NO`

这题是在平面上给的N个点，要求找到两组点(可以有一个点重复，不可以两个点都一样)曼哈顿距离相等。刚开始想复杂度要很大，以为要nlogn的解法。后来想了一段时间发现，曼哈顿距离最多只有2*M种可能，根据鸽笼原理必定会在2*M+1次之前产生重复。所以。。就暴力咯。

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
#define maxn 10000000
#define INF 1000000005
using namespace std;
bool mark[300005]={false};
int x[300005],y[300005];
int cal(int i,int j)
{
    return abs(x[i]-x[j])+abs(y[i]-y[j]);
}
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n,m;
        bool flag=false;
        scanf("%d%d",&n,&m);
        for(int i=0;i<n;i++)
        {
            scanf("%d%d",&x[i],&y[i]);
        }
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                int tmp=cal(i,j);
                if(mark[tmp]==false)
                {
                    mark[tmp]=true;
                }
                else
                {
                    flag=true;
                    break;
                }
            }
            if(flag==true)
                break;
        }
        if(flag==true)
            printf("YES\n");
        else
            printf("NO\n");
        memset(mark,false,sizeof(mark));
        memset(x,0,sizeof(x));
        memset(y,0,sizeof(y));
    }
    return 0;
}
```

***

# 1002 Permutation Bo

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 131072/131072 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0
Special Judge

### Problem Description
There are two sequences h1~hn and c1~cn. h1~hn is a permutation of 1~n. particularly, h0=hn+1=0.

We define the expression [condition] is 1 when condition is True,is 0 when condition is False.

Define the function f(h)=∑ni=1ci[hi>hi−1  and  hi>hi+1]

Bo have gotten the value of c1~cn, and he wants to know the expected value of f(h).
 

### Input
This problem has multi test cases(no more than 12).

For each test case, the first line contains a non-negative integer n(1≤n≤1000), second line contains n non-negative integer ci(0≤ci≤1000).
 

### Output
For each test cases print a decimal - the expectation of f(h).

If the absolute error between your answer and the standard answer is no more than 10−4, your solution will be accepted.
 

### Sample Input
`4
3 2 4 5
5
3 5 99 32 12`
 

### Sample Output
`6.000000
52.833333`

题意是给出一个系列1-n的排列组合，求每个位置峰点的概率乘上系数求和。刚开始以为是概率dp，懵逼了一段时间后发现。原来是有规律可循的。暴力写了几个小的概率后猜测了一发就过了。。其实是在最左右两端的对概率的贡献有1/2，在中间位置的，考虑 大中小 三种情况，可以知道在6种组合里只有 中大小 和 小大中 是对概率有贡献的，所以中间概率就是1/3。

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
#define maxn 10000000
#define INF 1000000005
using namespace std;
double a[1005];
int main()
{
    int n;
    while(~scanf("%d",&n))
    {
        double sum=0;
        for(int i=1;i<=n;i++)
        {
            scanf("%lf",&a[i]);
            if(i!=1&&i!=n)
                sum+=a[i];
        }
        printf("%.4lf\n",0.5*(a[1]+a[n])+(1.0/3.0)*sum);
        memset(a,0,sizeof(a));
    }
    return 0;
}
```

***

# 1010 Rower Bo

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 131072/131072 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0
Special Judge

### Problem Description
There is a river on the Cartesian coordinate system,the river is flowing along the x-axis direction.

Rower Bo is placed at (0,a) at first.He wants to get to origin (0,0) by boat.Boat speed relative to water is v1,and the speed of the water flow is v2.He will adjust the direction of v1 to origin all the time.

Your task is to calculate how much time he will use to get to origin.Your answer should be rounded to four decimal places.

If he can't arrive origin anyway,print"Infinity"(without quotation marks).
 

### Input
There are several test cases. (no more than 1000)

For each test case,there is only one line containing three integers a,v1,v2.

0≤a≤100, 0≤v1,v2,≤100, a,v1,v2 are integers
 

### Output
For each test case,print a string or a real number.

If the absolute error between your answer and the standard answer is no more than 10−4, your solution will be accepted.
 

### Sample Input
`2 3 3
2 4 3`
 

### Sample Output
`Infinity
1.1428571429`

这题就是高中物理竞赛里的经典的狗追兔子曲线。题意是在河对岸有一艘船以v1速度垂直向着原点开来，河流速度v2。行驶过程中始终指向原点，求到达时间。需要做一个坐标系转换，相当于原点以v2速度向一侧移动，船盯住原点开始前进。
源自华罗庚在《高等数学引论》第一卷第二分册第14章第8节“追踪问题”（p137-138）中所做的一种解答。当垂直的时候会有特例`(VcosΦ＋U)R＝(V2^2-U2^2)t＋aV`这里,V是狐狸的速度,U是兔子的速度,a是初始距离,R是狐狸和兔之间的距离（当狐狸追上兔子时R＝0）,t是追击所需的时间。
据此进一步推导可得`t＝aV/(V^2-U^2)`,兔子奔逃的距离`S=aVU/ (V^2-U^2)` ,狐狸追击的距离`S=aV^2/(V^2-U^2)`。
故当v1<=v2的时候就无法到达，注意要特判a=0的情况，输出0，在这里wa了一次。

```cpp
#include <stdio.h>
#include <math.h>

int main() {
    int a,v1,v2;
    while(~scanf("%d%d%d", &a,&v1,&v2)) {
        if(a==0) {
            printf("0\n");
            continue;
        }
        if(v1>v2) {
            double x = a*v1;
            x /= (v1*v1-v2*v2);
            printf("%lf\n", x);
        } else {
            printf("Infinity\n");
        }
    }
}
```

***

# 1003 Life Winner Bo

Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 131072/131072 K (Java/Others)
Total Submission(s): 0    Accepted Submission(s): 0


### Problem Description
Bo is a "Life Winner".He likes playing chessboard games with his girlfriend G.

The size of the chessboard is N×M.The top left corner is numbered(1,1) and the lower right corner is numberd (N,M).

For each game,Bo and G take turns moving a chesspiece(Bo first).At first,the chesspiece is located at (1,1).And the winner is the person who first moves the chesspiece to (N,M).At one point,if the chess can't be moved and it isn't located at (N,M),they end in a draw.

In general,the chesspiece can only be moved right or down.Formally,suppose it is located at (x,y),it can be moved to the next point (x′,y′) only if x′≥x and y′≥y.Also it can't be moved to the outside of chessboard.

Besides,There are four kinds of chess(They have movement rules respectively).

1.king.

2.rook(castle).

3.knight.

4.queen.

(The movement rule is as same as the chess.)

For each type of chess,you should find out that who will win the game if they both play in an optimal strategy.

Print the winner's name("B" or "G") or "D" if nobody wins the game.
 

### Input
In the first line,there is a number T as a case number.

In the next T lines,there are three numbers type,N and M.

"type" means the kind of the chess.

T≤1000,2≤N,M≤1000,1≤type≤4
 

### Output
For each question,print the answer.
 

### Sample Input
`4
1 5 5
2 5 5
3 5 5
4 5 5`
 

### Sample Output
`G
G
D
B`

这题出的我觉得很有意思，结合了博弈的知识。题意是给定一个N*M的棋盘，左上角为(1,1),右下角(n,m)。给定四种棋子，王，车，马，皇后。按照国际象棋的规则行动，唯一的限制是只能往左下角移动。其实这个限制非常的重要，是转化成博弈题目的关键所在。
首先考虑简单的车，只能向下和向右移动，其实可以转化成nim博弈。一开始有两堆石子，n和m，任意从一堆中取任意多棋子，谁先取完谁赢。那就很简单了，n=m，后手赢，n!=m,先手赢，因为先手总可以把局面变成n=m。
其次考虑王，从右下角开始往回推，可以得到每间隔2个是一个必胜态。故判断(n,m)是否等于(1+2i,1+2j)就行了。
接着考虑马，这个比较麻烦。可以手动推几个简单的点，得出规律，要注意，后手可以故意把局面导向平局。所以符合(3i,3i-1)或(3i-1,3i)的点是先手赢，符合(4+3i,4+3i)(对角线)的点是后手赢。
最后考虑皇后，这个是最坑的。我和成神推理了半天。得到了下面的表
![](http://oava57hou.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720160726230115.png)
但是并不会找规律TAT。后来习得技能后发现，这是一个简单的威佐夫博弈。判断是否符合威佐夫博弈的必败态的点就行了。

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
#define maxn 10000000
#define INF 1000000005
using namespace std;
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int p,n,m;
        scanf("%d%d%d",&p,&n,&m);
        if(p==1)
        {
            if((n-1)%2==0&&(m-1)%2==0)
                printf("G\n");
            else
                printf("B\n");
        }
        else if(p==2)
        {
            if(n==m)
                printf("G\n");
            else
                printf("B\n");
        }
        else if(p==3)
        {
            if((n%3==0&&n-1==m)||(m%3==0&&m-1==n))
                printf("B\n");
            else if((n-4)%3==0&&(m-4)%3==0&&n==m)
                printf("G\n");
            else
                printf("D\n");
        }
        else if(p==4)
        {
            if((int)(abs(n-m)*((sqrt(5.0)+1.0)/2.0))==min(n-1,m-1))
                printf("G\n");
            else
                printf("B\n");
        }
    }
    return 0;
}
```

第三场多校就先题解到这里，等以后技能等级提高了再做补充。

[2016多校联合训练第三场](http://acm.hdu.edu.cn/showproblem.php?pid=5752)(hdu题号从5752到5762)
