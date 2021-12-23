---
layout: post
title: 02. 자료구조_Queue
description: >
    Queue 자료구조
sitemap: false
hide_lase_modified: true
categories:
 - study
 - datastructure
---

# 02. 자료구조_Queue

* toc
{:toc .large-only}

## 1. Queue 자료구조란?

큐(queue)는 컴퓨터의 기본적인 자료 구조의 한가지로, 먼저 집어 넣은 데이터가 먼저 나오는 FIFO(First In First Out)구조로 저장하는 형식을 말한다. 영어 단어 queue는 표를 사러 일렬로 늘어선 사람들로 이루어진 줄을 말하기도 하며, 먼저 줄을 선 사람이 먼저 나갈 수 있는 상황을 연상하면 된다.

나중에 집어 넣은 데이터가 먼저 나오는 스택과는 반대되는 개념이다.        (출처: 위키백과)

## 2. Queue 구현 방법

#### 2-1. 배열 (Array)

~~~
1. 인덱스를 통한 접근이 간편하다.
2. 미리 할당 시 남는 메모리가 존재한다.
3. 비슷한 이유로 앞서 할당 받은 메모리를 초과하지 못한다.
~~~

#### 2-2. 링크드 리스트 (Linked-List)

~~~
1. 인덱스를 통한 접근이 불편하다.
2. 데이터 enqueue 시 데이터 크기만큼 메모리를 할당 받으므로 메모리 누수가 없다.
3. 따라서 메모리가 제한적인 환경에서 유리하다.
~~~

## 3. Array를 활용한 Queue 구현

#### 3-1. queue 자료구조 선언
~~~cpp
int queue[MAX];
int front, rear;
~~~

#### 3-2. queue init 함수

~~~cpp
void init_queue() {
	front = 0;
	rear = 0;
}
~~~

#### 3-3. queue put 함수

~~~cpp
int put(int k) {
	if ((rear + 1) % MAX == front) {
		printf("Queue overflow!!\n");
		return -1;
	}
	queue[rear] = k;
	rear = ++rear % MAX;
	return k;
}
~~~

#### 3-4. queue get 함수
~~~cpp
int get() {
	int j;

	if (front == rear) {
		printf("Queue underflow!!\n");
		return -1;
	}
	j = queue[front];
	front = ++front % MAX;

	return j;
}
~~~

#### 3-5. queue clear 함수
~~~cpp
void clear() {
	front = rear;
}
~~~

#### 3-6. main 함수
~~~cpp
void main() {
	int k;
	init_queue();
	put(3);
	put(6);
	put(9);
	put(1);
	put(6);
	put(3);
	print_queue();
	put(4);
	put(8);
	put(7);
	put(2);
	put(0);
	clear();
	get();
}
~~~
> 🔍 **실행 결과**

~~~Bash
3
6
9
1
6
3
Queue overflow!!
Queue overflow!!
Queue underflow!!
~~~

## 4. 링크드 리스트(Linked-List)를 활용한 Queue 구현

#### 4-1. queue 자료구조 선언
~~~cpp
typedef struct _dnode
{
	int key;
	struct _dnode* prev;
	struct _dnode* next;
}dnode;

dnode* dhead, * dtail;
~~~

#### 4-2. queue init 함수
~~~cpp
void init_queue()
{
	dhead = (dnode*)calloc(1, sizeof(dnode));
	dtail = (dnode*)calloc(1, sizeof(dnode));
	dhead->prev = dhead;
	dhead->next = dtail;
	dtail->prev = dhead;
	dtail->next = dtail;
}
~~~

#### 4-3. queue put 함수

~~~cpp
int put(int k)
{
	dnode* t;
	if ((t = (dnode*)malloc
	(sizeof(dnode))) == NULL)
	{
		printf("Out of memory !\n");
		return -1;
	}
	t->key = k;
	dtail->prev->next = t;
	t->prev = dtail->prev;
	dtail->prev = t;
	t->next = dtail;
	return k;
}
~~~

#### 4-4. queue get 함수
~~~cpp
int get()
{
	dnode* t;
	int k;
	t = dhead->next;
	if (t == dtail)
	{
		printf("Queue underflow\n");
		return -1;
	}
	k = t->key;
	dhead->next = t->next;
	t->next->prev = dhead;
	free(t);
	return k;
}
~~~

#### 4-5. print, clear 함수

~~~cpp
void clear_queue()
{
	dnode* t, * s;
	t = dhead->next;
	while (t != dtail)
	{
		s = t;
		t = t->next;
		free(s);
	}
	dhead->next = dtail;
	dtail->prev = dhead;
}
void print_queue()
{
	dnode* t;
	t = dhead->next;
	while (t != dtail) {
		printf("% -6d", t->key);
		t = t->next;
	}
}
~~~

#### 4-6. main 함수

~~~cpp
void main() {
	int k;
	init_queue();

	put(3);
	put(6);
	put(9);
	put(1);
	put(6);
	put(3);
	print_queue();
	put(4);
	put(8);
	put(7);
	put(2);
	put(0);
	clear_queue();
	get();
}
~~~
> 🔍 **실행 결과**

~~~bash
 3     6     9     1     6     3    Queue underflow
~~~

## 5. 정리

stack과 함께 자주 쓰일 자료구조.

 queue는 stack과 다르게 Doubly Linked-List로 구현했다. 

큰 의미는 없고 일반 linked-list로 똑같이 구현 가능하다.
