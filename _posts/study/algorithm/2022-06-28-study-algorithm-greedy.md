---
layout: post
title: 알고리즘 독학-그리디와 싸워 이기는 법
description: >
    그리디 알고리즘
hide_lase_modified: true
categories:
 - study
 - algorithm
---

# 알고리즘 독학-그리디와 싸워 이기는 법
~
* toc~
{:toc .large-only}

본 내용은 "이것이 취업을 위한 코딩테스트다"(나동빈 저) 서적의 내용을 참고합니다.

## 1. 그리디(Greedy) 알고리즘이란?

"탐욕법"이라는 이름으로도 불리며, '현재 상황에서 지금 당장 좋은 것만 고르는 방법'을 의미한다.
책에서 반가운 이름이 보였는데 바로 "다익스트라(Dijkstra) 알고리즘"이었다. 알고리즘 강의를 수강할 최소 비용 알고리즘 파트에서 본 기억이 있고, 알고리즘으로 구현해 본 경험이 있었는데 다익스트라 알고리즘이 바로 그리디 알고리즘의 일종이라고 한다.

## 2. 그리디(Greedy) 알고리즘 예시
알고리즘 예시 문제는 서적의 예시를 참고한다.
### 2-1. 거스름돈
>당신은 음식점의 계산을 도와주는 점원이다. 카운터에는 거스름돈으로 사용할 500원, 100원, 50원, 10원짜리 동전이 무한히 존재한다고 가정한다. 손님에게 거슬러 줘야할 돈이 N원일 때 거슬러 줘야할 동전의 최소 개수를 구하라. 단, 거슬러 줘야 할 돈 N은 항상 10의 배수이다.

언뜻보면 상당히 쉬운 알고리즘 문제인것 같은데, 해설을 보기 전에 해당 문제의 답을 코드로 구현해 보았다.

~~~cpp
int main() {
	int M, n = 0;
	scanf("%d", &M);
	if (M / 500) {
		n += M / 500;
		M %= 500;
	}
	if (M / 100) {
		n += M / 100;
		M %= 100;
	}
	if (M / 50) {
		n += M / 50;
		M %= 50;
	}
	if (M / 10) {
		n += M / 10;
	}
	printf("%d\n", n);
}
~~~

거스름돈을 걸러주는 문제의 경우, 동전의 우선순위가 명확해서 큰 동전부터 차례대로 카운트 하는 방식을 생각했다. 그리디 알고리즘의 개요는 "현재 상황에서 당장 좋은 선택을 하는 것"인데, 여기서 "당장 좋은 선택"은 무엇일까?
바로 "제일 큰 값의 동전을 선택하는 것"이라고 볼 수 있다.

해당 문제가 그리디 알고리즘으로 해결할 수 있는 이유가 바로 큰 단위의 동전을 우선으로 사용하되, 큰 단위의 동전이 작은 단위의 동전의 배수로 이루어져 있으므로 최소의 값을 갖게 하는 경우가 1개 밖에 없다.

예를 들어 900원을 거슬러줘야하는 상황에서 500원 300원 100원 동전이 있다면 위의 코드로는 (500 + 300 + 100)의 경우와 (300 + 300 + 300)의 두가지 경우가 나온다.
비슷한 문제로 BOJ 11047번 (동전 0) 문제가 있다.

![그림1](/assets/img/etc/greedy01.jpg)

### 2-2. 큰 수의 법칙

>동빈이의 큰 수의 법칙은 다양한 수로 이루어진 배열이 있을 때 주어진 수들을 M번 더하여 가장 큰 수를 만드는 법칙이다. 단, 배열의 특정한 인덱스에 해당하는 수가 연속해서 K번을 초과하여 더해질 수 없는 것이 이법칙의 특징이다.

예를 들어 순서대로 2, 4, 5, 4, 6으로 이루어진 배열이 있을 때 M이 8이고, K가 3이라고 가정하자. 이 경우 특정한 인덱스의 수가 연속해서 세 번까지만 더해질 수 있으므로 큰 수의 법칙에 따른 결과는 6 + 6 + 6 + 5 + 6 + 6 + 6 + 5인 46이 된다.

>단, 서로 다른 인덱스에 해당하는 수가 같은 경우에도 서로 다른 것으로 간주한다.

예를 들어 순서대로 3,4,3,4,3 으로 이루어진 배열이 있을 때 M이 7이고, K가 2라고 가정하자. 이 경우 두 번째 원소에 해당하는 4와 네 번째 원소에 해당하는 4를 번갈아 두 번씩 더하는 것이 가능하다. 결과적으로 4+4+4+4+4+4+4 인 28이 도출된다.

해당 문제에 대해서도 cpp 코드를 한번 작성해 보았다.

~~~cpp
int main() {
	int N, M, K;
	int* num;

	scanf("%d %d %d", &N, &M, &K);
	num = (int*)malloc(N * sizeof(int));
	int max = 0;
	int second = 0;
	for (int i = 0; i < N; i++) {
		scanf("%d", &num[i]);
		max = (num[i] >= num[max]) ? i : max;
	}
	for (int i = 0; i < N; i++) {
		second = (i != max && num[i] > num[second]) ? i : second;
	}

	printf("%d\n", (num[max] * K + num[second]) * (M / (K + 1)) + num[max] * (M % (K + 1)));

	return 0;
}
~~~
>✏️입력
~~~Bash
5 8 3
2 4 5 4 6
~~~
>🔍결과
~~~Bash
46
~~~

서적에서는 sort 알고리즘을 이용해서 1번째, 2번째 index를 이용했는데 max, second max값만 이용하는 경우이므로 2개 값의 index만 구해주는 방법으로 코딩했다.
예외는 따로 확인하지 않았는데 기본 예제에 대해서는 올바른 정답이 나왔다.

### 2-3. 숫자 카드게임

여러 개의 숫자 카드 중에서 가장 '높은' 숫자가 쓰인 카드 한 장을 뽑는 게임이다.
단, 게임의 룰을 지키며 카드를 뽑아야 하고 룰은 다음과 같다.
>1. 숫자가 쓰인 카드들이 N x M 형태로 놓여 있다. 이때 N은 행의 개수를 의미하며, M은 열의 개수를 의미한다.
>2. 먼저 뽑고자 하는 카드가 포함되어 있는 행을 선택한다.
>3. 그다음 선택된 행에 포함된 카드들 중 가장 숫자가 낮은 카드를 뽑아야 한다.
>4. 따라서 처음에 카드를 골라낼 행을 선택할 때, 이후에 해당 행에서 가장 숫자가 낮은 카드를 뽑을 것을 고려하여 최종적으로 가장 높은 숫자의 카드를 뽑을 수 있도록 전략을 세워야 한다.

순간 문제가 이해가 안갔는데 결론은 '높은' 숫자를 뽑아라, 대신 룰을 따르라. 가 요점인듯 하다.

~~~cpp
int main() {
	int N, M;
	int *card;
	scanf("%d %d", &N, &M);
	card = (int*)malloc(N * M * sizeof(int));
	for (int i = 0; i < N * M; i++) {
		scanf("%d", &card[i]);
	}
	int min, emax = 0;;
	for (int j = 0; j < N; j++) {
		min = INT_MAX;
		for (int i = 0; i < M; i++) {
			min = (min < card[j * M + i]) ? min : card[j * M + i];
		}
		emax = (min > emax) ? min : emax;
	}
	printf("%d", emax);
	return 0;

}
~~~
>✏️입력
~~~Bash
2 4
7 3 1 8
3 3 3 4
~~~
>🔍결과
~~~Bash
3
~~~

간단한 알고리즘이다. 행별로 min값을 구하고 min값끼리 비교해 max값을 찾는 문제였다. python에서는 min 함수를 쓰겠지만 cpp 환경에서는 그냥 for문 2개를 돌렸다. 굳이 카드 그림까지 가져오면서 설명을 했어야했나 할 정도로 간단한 알고리즘이라 넘어감.

###2-4. 1이 될 때까지
어떠한 수 N이 1이 될 때까지 다음의 두 과정 중 하나를 반복적으로 선택하여 수행하려고 한다. 단, 두 번째 연산은 N이 K로 나누어 떨어질 때만 선택할 수 있다.
>1. N에서 1을 뺀다.
>2. N을 K로 나눈다.

~~~cpp
int main() {
	int N, K;
	scanf("%d %d", &N, &K);
	int cnt = 0;
	while (N != 1)
	{
		if (N % K == 0) {
			N /= K;
		}
		else
			N--;
		cnt++;
	}
	printf("%d", cnt);
}
~~~
이런 코드를 빠르게 고민해보았는데 이것보다는 (1을 빼는 과정)을 (K로 나눈 나머지로 바로 빼는) 알고리즘이 좋을거같아서 하나 더 만들었다.
~~~cpp
int main() {
	int N, K;
	scanf("%d %d", &N, &K);
	int cnt = 0;
	while (N >= K)
	{
		cnt += N % K;
		N /= K;
		cnt++;
	}
	cnt += N - 1;

	printf("%d", cnt);
}
~~~
>✏️입력
~~~bash
100 7
~~~
>🔍결과
~~~bash
5
~~~
아마 큰 수의 경우 두 개의 알고리즘에서 시간 차이가 날 것이라 생각했다.

### 3. 그리디 알고리즘 끝.
'이것이 취업을 위한 코딩 테스트다(with 파이썬)' 서적의 첫번째 단원이 끝났다. BOJ에 단계별로 풀어보기 문제보다는 쉬운 문제들이어서 예제를 구현하는데 힘이 들진 않았는데, 쉬우니까 앞에 있었겠지.. 하는 생각이 들기도한다.

다만 지금(22년 7월 2일) 월요일부터 삼성 sds 대학생 알고리즘 특강을 2주간 수강해야해서 나머지는 15일부터 공부할 듯 하다.
