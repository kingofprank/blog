---
title: TrieTree(字典树，前缀树)
date: 2015-08-17 10:14:00
tags: [数据结构]
---
字典树属于树形数据结构之一，在省空间和时间上大有作为。
<!--more-->
# 关于TrieTree
TrieTree是一种有序树，通常的键是字符串，但键不是直接保存在节点中，每个节点的所有子孙都有共同的前缀，根节点一般对应空字符串。Trie可以看作是一个确定有限状态自动机(DFA)。

# TrieTree的实现
### 节点结构
```cpp
#define MAX 26//26个字母
struct trienode
{
	int ncount;//记录该节点出现过的次数
	struct trienode *next[MAX];
}memory[1000000];//自己设定空间大小
```

### 建树
```cpp
int allocp=0;
trienode *createtrie()
{
	trienode *temp=&memory[allocp++];
	temp->nconut=1;
	for(int i=0;i<MAX;i++)
		temp->next[i]=NULL;
	return temp;
}
trienode *root=createtrie();//根节点设为全局变量，减少调用次数
```

### 插入节点
```cpp
void inserttrie(char str[])//读入字符串
{
	trienode *temp=root;
	int i=0,k;
	while(str[i])
	{
		k=str[i]-'a';//计算位置
		if(temp->next[k])
		{
			temp->next[k]->ncount++;
		}
		else
		{
			temp->next[k]=createtrie();
		}
		temp=temp->next[k];
		i++;
	}
}
```

### 查询
```cpp
int searchtrie(char str[])//读入字符串
{
	if(root=NULL)
		return 0;
	trienode *temp=root;
	int i=0,k;
	while(str[i])
	{
		k=str[i]-'a';
		if(temp->next[k])
		{
			temp=temp->next[k];
		}
		else
			return 0;
		i++;
	}
	return tmp->ncount;//这个前缀出现了几次
}
```

# trietree的应用
* 用于串的快速检索
* 串的排序
* 最长公共前缀