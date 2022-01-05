---
layout: post
title: 00. 자료구조_Linked-List
description: >
    Linked-List 자료구조
hide_lase_modified: true
categories:
 - study
 - datastructure
---
# 00. 자료구조_Linked-List

* toc
{:toc .large-only}

## 1. Linked-List 자료구조란?

연결 리스트, 링크드 리스트(linked list)는 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료 구조이다. 이름에서 말하듯이 데이터를 담고 있는 노드들이 연결되어 있는데, 노드의 포인터가 다음이나 이전의 노드와의 연결을 담당하게 된다. (출처: 위키백과)

## 2. Linked-List 종류

#### 2-1. Singly Linked List

~~~cpp
// 노드 선언부
typedef struct _node
{
	int key;
	struct _node* next;
}node;

node* head, * tail;

// init 함수
void init_list()
{
	head = (node*)calloc(1, sizeof(node));
	tail = (node*)calloc(1, sizeof(node));
	head->next = tail;
	tail->next = tail;
}

// 노드 삭제 함수
int delete_node(int k)
{
	node* s, * p;
	p = head;
	s = p->next;
	while (s->key != k && s != tail) {
		p = p->next;
		s = p->next;
	}
	if (s != tail) {
		p->next = s->next;
		free(s);
		return 1;
	}
	else return 0;
}

// 노드 삽입 함수
node* insert_node(int t, int k)
{
	node* s, * p, * r;
	p = head;
	s = p->next;
	while (s->key != k && s != tail) {
		p = p->next;
		s = p->next;
	}
	if (s != tail) {
		r = (node*)calloc(1, sizeof(node));
		r->key = t;
		r->next = s->next;
		s->next = r;
		printf("%d insert after list(key : %d)\n", t, k);
	}
	return p->next;
}
~~~
#### 2-2. Doubly Linked List
#### 2-3. Circular Linked List

## 3. 정리

Tree구조, stack, queue 모든 자료 구조형에서 기본이 되는 linked-list,

간단하게 짚고 넘어감. 
