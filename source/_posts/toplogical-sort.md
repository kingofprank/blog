---
title: 拓扑排序
date: 2015-8-18 12:48:00
tags: [图论]
---
刚学到了一招关于拓扑排序的东西，就来兴致写这个了。
<!--more-->
# 关于拓扑排序

# 拓扑排序的实现
实现有分储存实现和算法实现，储存实现有用邻接矩阵和邻接表两种，算法实现我这里就列举出来用两重for循环和用队列实现的两种，本质上都是BFS，但其实还有一种是用DFS来实现，觉得可能会爆栈，以后再学吧。

## 邻接矩阵
没什么好讲的，用起来十分方便，不爆空间的时候很好用。
```cpp
构造：
int map[maxn][maxn];//最普通的邻接矩阵

struct node
{
	int nature;//记录属性
}
node map[maxn][maxn];//带属性的邻接矩阵

储存：
map[i][j]=1;//直接存

```
## 邻接表
压缩存储大杀器，瞬间就下降了存储量了，我用的是vector来实现的，也可以手写链表，也可以用一个数组来存，需要手写一个head[]指向边。
```cpp
vector<int>map[maxn];

struct node
{
	int j,nature;
}
vector<node>map[maxn];//带属性的邻接表

普通储存：
map[i].push_back(j);//注意存下来的**值**是j，实际上的位置不是第j个位置，要从头找

普通的查询只需要
for(i=0;i<map[temp].size();i++)
cout<<temp<<map[temp][i]<<endl;

多属性存储：
node temp;
temp.j=j;temp.nature=nature;//赋值
map[i].push_back(temp);//压进去

对于这种多属性的数组来说查询要用
for(i=0;i<map[temp].size();i++)
cout<<temp<<map[temp][i].j<<map[temp][i].nature<<endl;
```

## 队列实现
自从学会这个以后妈妈再也不用担心我背又长又臭的模板了,用优先队列解决输出顺序问题，用普通队列解决是否有环的问题。

1.队列的设定
```cpp
priority_queue<int,vector<int>,cmp>q;//可以改成自己设定的属性

这里可以用<functional>这个库解决普通类型的排序问题
可以用 greater<int> 从小到大排序，less<int>从大到小排序
默认的是从大到小的大根堆。

或者自写排序，在struct类型里面自己写
struct node
{
	int x;
	friend bool opreator <(node a,node b)//友元函数，只能重载小于号
	{
		return a.x<b.x;//这样是从大到小，也就是默认状态
	}
}
```
2.队列排序
```cpp
void topsort()
{
	for(i=1;i<=n;i++)
		if(degree[i]==0)
			q.push(i);
	while(!q.empty())
	{
		temp=q.top();//注意优先队列只提供top(),普通队列提供q.front()
		//output[cot++]=temp;把要输出的存下来，控制各式再输出
		q.pop();
		for(i=0;i<map[temp].size;i++)
			if(--degree[map[temp][i]]==0)
				q.push(map[temp][i]);
	}
}
```
这里要提醒一下，如果要判有没有重边的话，只能for一遍，比如
```cpp
scanf("%d%d",&a,&b);
for(i=0;i<map[a].size();i++)
	if(map[a][i]==b)
		{mark=1;break;}
if(!mark)
	map[a].push_back(b);
```

以上就是暂且学到的东西，之后学到会继续更新

## 循环实现



