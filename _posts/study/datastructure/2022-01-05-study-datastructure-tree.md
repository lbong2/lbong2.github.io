---
layout: post
title: 03. 자료구조_Tree
description: >
    Tree 자료구조
hide_lase_modified: true
categories:
 - study
 - datastructure
---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8919104066540378"
     crossorigin="anonymous"></script>
     
# 00. 자료구조_Tree

* toc
{:toc .large-only}

## 1. Tree 자료구조란?

트리 구조(tree 構造, 문화어: 나무구조)란 그래프의 일종으로, 여러 노드가 한 노드를 가리킬 수 없는 구조이다. 간단하게는 회로가 없고, 서로 다른 두 노드를 잇는 길이 하나뿐인 그래프를 트리라고 부른다.

트리에서 최상위 노드를 루트 노드라고 한다. 또한 노드 A가 노드 B를 가리킬 때 A를 B의 부모 노드, B를 A의 자식 노드라고 한다. 자식 노드가 없는 노드를 잎 노드라고 한다. 잎 노드가 아닌 노드를 내부 노드라고 한다.
(출처: 위키백과)

## 2. Tree 구현
#### 2-1. Tree 선언
~~~cpp
typedef struct _node
{
	char key;
	struct _node* left;
	struct _node* right;
}node;
node* head, * tail;
~~~

#### 2-2. init 함수
~~~cpp
void init_tree()
{
	head = (node*)calloc(1, sizeof(node));
	tail = (node*)calloc(1, sizeof(node));
	head->left = tail;
	head->right = tail;
	tail->left = tail;
	tail->right = tail;
}
~~~

#### 2-3. setTree 함수(노드 삽입)
~~~cpp
node* set_tree(node* t, int k)
{
	node* tmp = (node*)malloc(sizeof(node));
	tmp->key = k;
	if (t->left == tail)
	{
	t->left = tmp;
	tmp->left = tail;
	tmp->right = tail;
	}
	else if (t->right == tail) {
	t->right = tmp;
	tmp->left = tail;
	tmp->right = tail;
	}

	else
		printf("Node's children is full\n");
	return tmp;
}
~~~

## 3. 정리

간단하게 Tree구조 구현을 했다.

후에 포스팅할 정렬 알고리즘이나, 여러 다른 종류의 Tree구조에서 매우 빈번하게 활용되는 자료구조.

또한 Tree 탐색 또한 알고리즘 카테고리에서 다룰 예정.
