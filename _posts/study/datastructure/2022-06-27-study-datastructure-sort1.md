---
layout: post
title: 04. 정렬 알고리즘_1
description: >
    정렬 알고리즘
hide_lase_modified: true
categories:
 - study
 - datastructure
---
* toc
{:toc .large-only}

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8919104066540378"
     crossorigin="anonymous"></script>

# 04. 정렬 알고리즘_1
방학이 되어서 미뤘던 포스팅을 해보려 한다.. 엄밀히 말하면 정렬 "알고리즘"이라 알고리즘 카테고리에 올려야하는데 알고리즘 탭은 서적 리뷰형식으로 쓰고 나중에 카테고리 정리를 해야할거같다.. ㅠㅠ 일이 2배

## 1. 정렬 알고리즘이란?
이름에서 알 수 있듯 말그대로 정렬 하는 방법이다. 간단하게 데스크탑 파일 탐색기를 통해 파일을 확인해보면 우클릭 혹은 탐색기 상단에 정렬 기준을 고르는 탭을 확인할 수 있다. 이름순, 수정 날짜순, 크기순 등등..
사람들은 이러한 지표를 눈으로 보고 순서를 매길 수 있지만 이 멍청한 컴퓨터는 기준을 정하고 기준에 맞는 알고리즘을 설계해 주어야 정렬을 할 수 있다. 당연히 속도는 컴퓨터가 빠르다

## 2. 정렬 알고리즘 종류 및 구현
기본적으로 모든 알고리즘 함수의 파라미터는 같다. (포인터, 크기)
그리고 방법 또한 pivot을 1개 사용하냐, 2개 사용하냐, 바로 옆 인자랑 비교하냐, 랜덤으로 비교하냐 등 방법이 다를 뿐 기본적인 뼈대는 비슷하다고 느껴진다.
해당 강의를 수강할 때, int 자료형 뿐만아니라 모든 자료형에 정렬을 사용할 수 있는 일반화 정렬코드(General Sort)를 제공 받고 제공 받은 코드를 바탕으로 다른 정렬 코드를 일반화하는 과제를 수행한 적이 있어 일반화 코드도 가지고 있지만, 구글링을 통한 복붙을 막기 위해 일반화 코드는 1개만 리뷰할 예정이다.
### 2-1. Bubble Sort(거품 정렬)
참고로 이름은 정렬되는 모습이 마치 거품이 올라오는 모습을 닮아서 지어졌다고 한다.
~~~cpp
void bubble_sort(int* a, int n) {
	int i, j, t;
	for (i = 0; i < n - 1; i++) {
		for (j = 1; j < n - i; j++) {
			if (a[j - 1] < a[j]) {
				t = a[j - 1];
				a[j - 1] = a[j];
				a[j] = t;
			}
		}
	}
}
int intcmp(const void* a, const void* b)
{
	return (*(int*)a - *(int*)b);
}
void gen_bubble_sort(void* base, int nelem, int width, int(*fcmp)(const void*, const void*)) {
	int i, j, s;
	void* t;
	t = malloc(width);
	for (i = 0; i < nelem; i++) {
		s = 1;
		for (j = 1; j < nelem - i; j++) {
			if (fcmp((char*)base + (j - 1) * width, (char*)base + j * width) > 0) {
				memcpy(t, (char*)base + (j - 1) * width, width);
				memcpy((char*)base + (j - 1) * width, (char*)base + j * width, width);
				memcpy((char*)base + j * width, t, width);
				s = 0;
			}
		}
		if (s == 1) break;
	}
}

~~~

Bubble sort는 여러 원소들 중 인접한 두 원소를 비교하여 정렬하는 알고리즘이다. 코드에서도 볼 수 있듯이, 이게 맞든 틀리든 모든 경우를 다 계산하기 때문에 시간복잡도는 $$T(n) = O(n^2)$$ 이다.

아래의 일반화 코드를 잠시 살펴보면 결론적으로 원소를 교환하는 과정에서 memcpy가 사용된 것을 제외하면 나머지는 같다.

### 2-2. Insertion Sort(삽입 정렬)
~~~cpp
void insert_sort(int* a, int N)
{
	int i, j, t;
	for (i = 1; i < N; i++)
	{
		t = a[i];
		j = i;
		while (j > 0 && a[j - 1] > t)
		{
			a[j] = a[j - 1];
			j--;
		}
		a[j] = t;
	}
}
void indirect_insert_sort(int* a, int* index, int N)
{
	int i, j, t;
	for (i = 0; i < N; i++)
		index[i] = i;
	for (i = 1; i < N; i++)
	{
		t = index[i];
		j = i;
		while (j > 0 && a[index[j - 1]] > a[t])
		{
			index[j] = index[j - 1];
			j--;
		}
		index[j] = t;
	}
}
~~~
이번엔 두 가지의 코드를 준비했는데 별 다른 부분은 없다. direct와 indirect의 차이점은 원소를 직접 바꾸냐, 아니면 index의 순서를 반환하느냐의 차이이다.

| --- |[0]|[1]|[2]|[3]|[4]|[5]|
| --- | --- | --- | --- | --- | --- | --- |
|target array|3|4|2|1|6|5|
|index array |3|2|0|1|5|4|  

이와 같이 index array의 값을 target array의 index에 넣어 순차적으로 출력해보면 정렬된 결과를 얻을 수 있을것이다.

insertion sort는 각각의 원소를 차례대로 pivot으로 설정하고 해당 원소가 어느 위치에 정렬되어야 할지 탐색한 뒤 그 위치에 '삽입'한다. 시간 복잡도는 위의 bubble sort와 같지만 이것은 둘다 최악의 경우이고, bubble에 비해 빠르다는 장점이 있다.
또한 이것을 위와 같이 배열 index 처리를 통해 구현할 경우 배열의 다량의 원소들을 밀고 당기는 과정에 연산과정이 복잡해지는데 이것은 memcpy를 통해 빠른 연산을 시행할 수 있다.
예를 들어
~~~cpp
memcpy ( data+i, data+i+1, sizeof(*data) * (j-i) );
~~~
(출처 : 위키백과)
이런 식으로 memcpy를 사용한다면 배열에서 내가 원하는 바이트 수 만큼 밀어낼 수 있다.

시간복잡도는 $$T(n) = O(n^2)$$ 이다.

### 2-3. Selection Sort(선택 정렬)
~~~cpp
void select_sort(int* a, int N)
{
	int min;
	int min_idx;
	int x, y;
	for (y = 0; y < N - 1; y++) {
		min_idx = y;
		min = a[y];
		for (x = y + 1; x < N; x++) {
			if (min > a[x]) {
				min = a[x];
				min_idx = x;
			}
		}
		a[min_idx] = a[y];
		a[y] = min;
	}
}
~~~
선택 정렬은 정해진 규칙에 따라 최소 혹은 최대의 원소를 찾고 해당 원소를 맨 앞 원소와 교체하는 방식으로 정렬이 진행된다. 방법을 한줄로 요약할 수 있을만큼 간단하고 코드도 이해하기 쉽다.
개선점으로는 탐색 과정에서 최대 최소를 한꺼번에 찾아 탐색 횟수를 절반으로 줄이는 방식이나,
한번 탐색시 최소, 최대 값이 여러개라면 한꺼번에 정렬하는 방식을 채택할 수 있다.
시간복잡도는 $$T(n) = O(n^2)$$ 이다.

### 2-4 Shell Sort(쉘 정렬)
~~~cpp
void shell_sort(int* a, int N)
{
	int i, j, k, h, v;
	for (h = N / 2; h > 0; h /= 2) {
		for (i = 0; i < h; i++) {
			for (j = i + h; j < N; j += h) {
				v = a[j];
				k = j;
				while (k > h - 1 && a[k - h] > v) {
					a[k] = a[k - h];
					k -= h;
				}
				a[k] = v;
			}
		}
	}
}
~~~
삽입 정렬을 보완한 정렬 방식이며, 방식은 다음과 같다.
>1. 데이터를 십수 개 정도 듬성듬성 나누어서 삽입 정렬한다.
>2. 데이터를 다시 잘게 나누어서 삽입 정렬한다.
>3. 이렇게 계속 하여 마침내 정렬이 된다.

(출처:위키백과)
결국 배열을 여러개의 군집으로 나누어 삽입 정렬을 진행한다는 것이다.
위의 코드에서 h가 군집의 크기로 이것은 계속 2로 나눠진다.

예를 들어
5,4.7,6,8,1,2,3이라는 수열이라면<br>

5 8<br>
4 1<br>
7 2<br>
6 3<br><br>
과 같이 2개의 열로 만든다. 이것을 행끼리 정렬한다.<br>
5 8<br>
1 4<br>
2 7<br>
3 6<br>
따라서 5,1,2,3,8,4,7,6 이라는 행렬이 1차 정렬을 통해 만들어진다. (h =/ 2)
이것을 다시
5 2 8 7<br>
1 3 4 6<br>
과 같이 4개의 열로 만든다. 이것을 행끼리 정렬한다.<br>
2 5 7 8<br>
1 3 4 6<br>
따라서 2,1,5,3,7,4,8,6 이라는 행렬이 2차 정렬을 통해 만들어진다. (h =/ 2)
h가 1이 되었으므로
2 1 5 3 7 4 8 6 의 행렬을 삽입 정렬로 정렬한다.<br>
1 2 3 4 5 6 7 8<br>
각 원소가 멀리 이동하지 않은 모습을 볼 수 있다.
말로는 설명이 어렵지만 위와 같이 2차원으로 숫자를 나타내니까 설명이 편하다.
하지만 여전히 시간복잡도는 $$T(n) = O(n^2)$$ 이다.
