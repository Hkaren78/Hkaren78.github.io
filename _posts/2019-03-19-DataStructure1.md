---
layout:     post
title:      "链表与指针"
subtitle:   ""
author:     "HK"
header-img: "img/DataStructure.jpg"
catalog: true
tags:
    - 数据结构
---

>一直没理清指针的一些知识点，整理一下~

## 链表中插入指针

在链表中插入结点只需要修改指针（在第i个结点之前插入元素，修改的是第i-1个结点的指针）。

![等待网络加载图片···](https://github.com/Hkaren78/Hkaren78.github.io/raw/master/img/in-post/DataStructure/zhizhen.png)

如图所示，在单链表中第i个节点之前进行插入的基本操作为：找到线性表中的第i-1个结点，然后先进行步骤①（e->next=ai）,再进行步骤②（ai-1->next=e）；反之，如若先将ai-1->next指针指向e,会覆盖掉ai中的数据,造成数据缺失。

```c
Status ListInsert_L(LinkList L,int i,ElemType e){
	//L代表线性链表，i是插入的位置，e是值

	//即L为带头结点的单链表的头指针，本算法在链表中第i个结点之前插入新的元素e

	p=L;j=0;
	//可能会在第一个元素之前插入元素，所修改的是头结点的指针，故p的初值为头结点

	while (p&&j<i-1)
	{
	//P不为空，寻找到第i个结点

		p=p->next;
		++j;
	}
	if(!p||j>i-1) return ERROR;//i大于表长或者小于1，插入位置不存在、

	s=(LinkList)malloc(sizeof(LNode));//为插入的值分配新结点s

	s->data=e;
	s->next=p->next; p->next=s;//插入

	return OK;	
}//ListInsert_L
```
