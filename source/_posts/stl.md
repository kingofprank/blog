---
title: STL库
date: 201-08-19 13:16:00
tags:
---
STL大法好，不论速度还是简单程度，以及安全性都是相当的可靠的。认真学习库！！
<!--more-->

# STL库
STL(Standard Template Library)，标准模板库，惠普实验室开发的一系列软件的统称。它是由Alexander Stepanov、Meng Lee和David R Musser在惠普实验室工作时所开发出来的。STL的目的是标准化组件，这样就不用重新开发，可以使用现成的组件。STL现在是C++的一部分。

在C++标准中，STL被组织为下面的17个头文件：<algorithm>、<deque>、<functional>、<iterator>、<array>、<vector>、<list>、<forward_list>、<map>、<unordered_map>、<memory>、<numeric>、<queue>、<set>、<unordered_set>、<stack>和<utility>。

STL可分为容器(containers)、迭代器(iterators)、空间配置器(allocator)、配接器(adapters)、算法(algorithms)、仿函数(functors)六个部分。


# vector
所谓的动态数组库就是以链表为基础构建的，可以随意的插入和删除结点，充分利用空间，经常用它来做邻接表(其实是我习惯，也有用链式前向星的)。

一般来说构建数组都是用以下方式
```cpp
vector<int>G[maxn];//普通的邻接表

struct edge{
	int to,cost;
}
vector<edge>G[maxn];//带属性的邻接表
```
初始化根据有向和无向用插入
```cpp
G[a].push_back(b);
G[b].push_back(a);
```
注意！有时候邻接表要判重边，自环。自环好判，重边只能遍历判
```cpp
for(i=0;i<G[temp].size();i++)
	if(G[temp][i]==b)
		{mark=1;break;}
if(!mark){
	.....}
```
清空的时候根据情况
```cpp
for(i=0;i<=n;i++)
	G[i].clear();
```
vector一般只会用到这些，已经很足够了，不论是初始化，查找定位，还是删除，这些都是基础的操作。但是其实vector还有一些很方便的接口，后续逐渐补充。

# queue
该头文件下最常用的就是priority_queue和queue这两个了，优先队列和普通队列是BFS的神器啊，本质也是链表作的，所以如果频繁的删减其实速度还是没有手动模拟的快的。

# stack

# deque

# map
map其实是一种关联容器，存储相结合形成的一个关键值和映射值的元素，值的类型为`pair<const Key,Data>`,一般写法是`map<Key,Data,Compare,Alloc>G`，map是由红黑树而作，用来方便的储存和检索，在map中插入和删除对象，不会使任何迭代器失效，仍然指向原来的元素，除非删除的是正被指向的元素的时候才会失效。

## 模板参数：
key：关键值的类型。在map对象中的每个元素是通过该关键值唯一确定元素的。

T：映射值的类型。在map中的每个元素是用来储存一些数据作为其映射值。

compare：Comparison类：A类键的类型，它有两个参数，并返回一个bool。表达comp（A，B），comp是这比较类A和B是关键值的对象，应返回true，如果是在早先的立场比B放置在一个严格弱排序操作。这可以是一个类实现一个函数调用运算符或一个函数的指针（见一个例子构造）。默认的对于<KEY>，返回申请小于操作符相同的默认值（A <B）。
Map对象使用这个表达式来确定在容器中元素的位置。以下这个规则在任何时候都排列在map容器中的所有元素。

map可以将插入的int,char*类型按照数字顺序，字典序排序，写法为
```cpp
struct cmp
{
	bool operator()(const char* s1,const char* s2)
	{
		return strcmp(s1,s2)<0;//从小到大按照字典序
	}
}
map<const char*,int,cmp> G;
```

Allocator：用于定义存储分配模型分配器对象的类型。默认情况下，分配器类模板，它定义了最简单的内存分配模式，是值独立的。

## 使用
### map的遍历
有两种遍历形式，正向和反向
```cpp
map<Key,Data>::iterator it;//正向迭代器
for(it=G.begin();it!=G.end();it++){...}

map<Key,Data>::reverse_iterator it;//反向迭代器
for(it=G.rbegin();it!=rend();it++){...}//注意此处还是it++，这个只是一个对迭代器的重载，并不是真正的自加

it->first;//指向Key
it->second;//指向Data
```
### map的插入
```cpp
G['a']=100;

G.insert(std::pair<char,int>('a',100));
```

# set

# list



