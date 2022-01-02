---
layout: post
title: 01. 자료구조_Stack
description: >
    Stack 자료구조
hide_lase_modified: true
categories:
 - study
 - datastructure
---

# 01. 자료구조_Stack

* toc
{:toc .large-only}

## 1. Stack 자료구조란?

스택(stack)은 제한적으로 접근할 수 있는 나열 구조이다. 그 접근 방법은 언제나 목록의 끝에서만 일어난다. 끝먼저내기 목록(Pushdown list)이라고도 한다.        (출처: 위키백과)

## 2. Stack 구현 방법

#### 2-1. 배열 (Array)

~~~
1. 인덱스를 통한 접근이 간편하다.
2. 미리 할당 시 남는 메모리가 존재한다.
3. 비슷한 이유로 앞서 할당 받은 메모리를 초과하지 못한다.
~~~

#### 2-2. 링크드 리스트 (Linked-List)

~~~
1. 인덱스를 통한 접근이 불편하다.
2. 데이터 push 시 데이터 크기만큼 메모리를 할당 받으므로 메모리 누수가 없다.
3. 따라서 메모리가 제한적인 환경에서 유리하다.
~~~

## 3. Array를 활용한 Stack 구현

#### 3-1. stack 자료구조 선언
~~~cpp
int stack[MAX];
int top;
~~~

#### 3-2. stack init 함수

~~~cpp
void init_stack()
{
	top = -1;
}
~~~

#### 3-3. stack push 함수

~~~cpp
int push(int t) {
	if (top >= MAX - 1)
	{
		printf("Stack overflow !!!\n");
		return -1;
	}
	stack[++top] = t;
	return t;
}
~~~

#### 3-4. stack pop 함수
~~~cpp
int pop() {
	if (top < 0) {
		printf("Stack underflow !!!\n");
		return -1;
	}
	return stack[top--];
}
~~~

#### 3-5. main 함수
~~~cpp
void main() {
	int k;
	init_stack();
	push(3);
	push(6);
	push(9);
	push(1);
	push(6);
	push(3);
	for (int i = top; i > -1; i--) {
		printf("%d\n", stack[i]);
	}
	push(4);
	push(8);
	push(7);
	push(2);
	push(0);
	init_stack();
	pop();
}
~~~
> 🔍 **실행 결과**

~~~Bash
3
6
1
9
6
3
Stack overflow !!!
Stack underflow !!!
~~~

## 4. 링크드 리스트(Linked-List)를 활용한 Stack 구현

#### 4-1. stack 자료구조 선언
~~~cpp
typedef struct _node
{
	int key;
	struct _node* next;
}node;

node* head, * tail;
~~~

#### 4-2. stack init 함수
~~~cpp
void init_stack()
{
	head = (node*)calloc(1, sizeof(node));
	tail = (node*)calloc(1, sizeof(node));
	head->next = tail;
	tail->next = tail;
}
~~~

#### 4-3. stack push 함수

~~~cpp
int push(int k)
{
	node* t;
	if ((t = (node*)malloc(sizeof(node))) == NULL)
	{
		printf("Out of memory !\n");
			return -1;
	}
	t->key = k;
	t->next = head->next;
	head->next = t;
	return k;
}
~~~

#### 4-4. stack pop 함수
~~~cpp
int pop()
{
	node* t;
	int k;
	if (head->next == tail) {
		printf("Stack underflow!\n");
		return -1;
	}
	t = head->next;
	k = t->key;
	head->next = t->next;
	free(t);
	return k;
}
~~~

#### 4-5. visit, clear 함수

~~~cpp
void clear() // all node delete
{  
	node* t, * s;
	t = head->next;

	while (t != tail) {
		s = t;
		t = t->next;
		free(s);
	}
	head->next = tail;
}
void visit(node* t) // present node printing
{
	printf("%d is visited\n", t->key);
}
~~~

#### 4-6. main 함수

~~~cpp
void main() {
	int k;
	node* t;
	init_stack();
	push(3);
	push(6);
	push(9);
	push(1);
	push(6);
	push(3);
	t = head->next;
	while (t != tail) {
		printf("%d\n", t->key);
		t = t->next;
	}
	push(4);
	push(8);
	push(7);
	push(2);
	push(0);
	clear();
	pop();
}
~~~
> 🔍 **실행 결과**

~~~bash
3
6
1
9
6
3
Stack underflow!
~~~

## 5. 정리

앞으로 코딩테스트나 여러 프로젝트에서 쓰이게 될 자료구조,

Array를 활용한 구현방법도 있지만, Linked-List 방식이
내가 직접 디자인한 구조체를 다룰 때 조금 더 편리한 방식.

앞으로 정렬이나 검색 알고리즘도 다룰텐데, 아마 linked-list방식만 다룰 예정.
