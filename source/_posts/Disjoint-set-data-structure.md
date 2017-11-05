---
title: 并查集
date: 2015-8-12 19:07:00
tags: [数据结构]
---

自从有了并查集，妈妈再也不用担心我分类了。。
<!--more-->

并查集是用来分类的一种数据结构，其主要的优化有两种。
一种是**优化并查集的查询，叫路径压缩**，有递归和迭代两种版本。
一种是**优化并查集的合并，叫按秩合并**，用rank[]数组记录下代表元素子树的最大可能高度，把矮树挂到高树下面作为孩子，尽量减少树的高度。

# 实现
1.并查集的初始化
```cpp
for(i=0;i<=n;i++)
{
	f[i]=i;
	rank[i]=0;
}
```

2.并查集的查询
* 递归版
```cpp
int find(int x){
	if(x!=f[x])
		f[x]=find(f[x]);//路径压缩
	return f[x];
}
```
* 递归简单版
这个版本就一行，直接路径压缩加合并，代码方便。
```cpp
int find(int x){
	return f[x]==x?x:f[x]=find(f[x]);
}
```
* 非递归版
防止爆栈。
```cpp
int find(int x){
	int root,k,temp;
	root=x;
	while(root!=f[root])
		root=f[root];//查找根节点
	temp=x;
	while(temp!=root)//如果还没到根节点
	{
		k=f[temp];
		f[temp]=root;//把每个节点都压缩到根节点
		temp=k;//找这个节点的父亲继续操作
	}
	return root;
}
```

3.并查集的合并
* 普通合并
```cpp
int union(int x,int y){
	int f1=find(x);
	int f2=find(y);
	if(f1!=f2)
		{f[f2]=f1;return 1;}//之前未合并，现在合并
	return 0;//已经是合并过的
}
```
* 按秩合并
```cpp
int union(int x,int y){
	int f1=find(x);
	int f2=find(y);
	if(x==y) return 0;//已经在一个集合中
	else{
		if(rank[f1]>=rank[f2]){
			f[f2]=f1;
			rank[f1]+=rank[f2];
		}//f2子树比f1子树矮，将f2挂在f1上，更新rank
		else{
			f[f1]=f2;
			rank[f2]+=rank[f1];
		}
		return 1;//挂上去
	}
}
```
对于大部分的并查集问题来说，路径压缩就已经足够了，不需要按秩合并，有的时候是因为防止爆栈所以需要用非递归版的并查集或者用按秩合并。但是极端情况下，同时用上路径压缩和按秩合并会减少更多的时间。

4.并查集的删除
今日被一道[fzu2155](http://acm.fzu.edu.cn/problem.php?pid=2155)给坑到了，好吧，这个思路还是很清楚的，涨姿势了并查集的删除需要用一个real[]数组来记录一下真实位置在哪，如果删除一个点就把这个点的real位置移到最后端，这样在不影响原来并查集的基础上还能进行并查集的删除操作。
之后对于并查集的操作都是扔进去**real[x]位置**进行合并，查询。

```cpp
for(i=0;i<n;i++)
    f[i]=real[i]=i;//real[]和f[]一样的初始化
int k=n;//k指向末尾
if(cmd=="union")
{
    scanf("%d%d",&a,&b);
    union(real[a],real[b]);//合并的时候合并真实位置
}
else if(cmd=="delete")
{
    scanf("%d",tar);
    f[k]=k;//将末尾的并查集f[]数组也初始化了
    real[tar]=k++;//把要删除的该点real位置挂上去，也就是移动到最后
}
```

# 应用

## 关系并查集(权重并查集)
不得不说到poj上一道经典的并查集题目[食物链](http://acm.pku.edu.cn/JudgeOnline/problem?id=1182)，推测三种动物之间的关系，然后将并查集储存的元素加上储存父亲结点和相互之间的关系(也就是存边)，推出两个式子。然后就可以判断了，下面贴出代码。

```cpp
#include<iostream>
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<cmath>
#include<queue>
#include<stack>
#include<map>
using namespace std;
struct node
{
    int parent;
    int relation;
}f[50005];
int finding(int x)
{
    if(x!=f[x].parent)
    {
        int temp;
        temp=f[x].parent;//必须加temp来存
        f[x].parent=finding(temp);//路径压缩找父亲
        f[x].relation=(f[temp].relation+f[x].relation)%3;///更新
        //root->x=root->x.father+x.father->x
        return f[x].parent;
    }
    return x;
}
int main()
{
    int n,k,cot=0,i,x,y,root1,root2,d;
    scanf("%d%d",&n,&k);
    for(i=0;i<=n;i++)
        {f[i].parent=i;f[i].relation=0;}
    while(k--)
    {
        scanf("%d%d%d",&d,&x,&y);
        if(x>n||y>n)
            {cot++;continue;}
        root1=finding(x);
        root2=finding(y);
        if(root1!=root2) {//不在同一集合中,把root2挂到root1上
                f[root2].parent=root1;//更新root1->root2的关系
                f[root2].relation=(f[x].relation+(d-1)+(3-f[y].relation))%3;
                //root1->root2=root1->x+x->y+y->root2
            }
        else {//在同一集合里
            if(d==1) {
                if(f[x].relation!=f[y].relation)
                    {cot++;continue;}
            }
            else {
                if((3-f[x].relation+f[y].relation)%3!=1)///x->y=x->root+root->y
                    {cot++;continue;}
            }
        }
    }
    printf("%d\n",cot);
    return 0;
}
```

## Kruskal算法
这是图论中计算最小生成树的算法，将在图论文章中讲述。