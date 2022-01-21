---
layout: post
title: Image Gradient
description: >
    Image Gradient
hide_lase_modified: true
categories:
 - study
 - opencv
---

# Image 각 방향 Gradient 구하기

언뜻보면 연속적으로 보이는 사진도 픽셀값으로 이루어져 있다.

어린시절 TV나 컴퓨터의 CRT모니터를 아주 가까이서 보면 신기하게 파랑, 초록, 빨강색으로 이루어져 있던것이 기억난다.

어쨌든 이러한 픽셀값들 사이의 합, 차, 평균, 거리 등은 이미지를 분석하는데 아주 중요한 특징이다.

따라서 이번 포스트에서는 이미지 픽셀 방향별 Gradient를 구하는 코드를 구현해 보았다.

## 1. Grad function
~~~cpp
void Grad(float* img, float* X, float* Y, int c, int r) {
	// x방향 Gradient
	for (int i = 0; i < c; i++)
		for (int j = 0; j < r; j++) {
			if (i == 0)
				X[j * c + i] = img[j * c + i];
			else if(i == c - 1)
				X[j * c + i] = -img[j * c + i];
			else
				X[j * c + i] = img[j * c + i + 1] - img[j * c + i - 1];
		}
	// y방향 Gradient
	for (int i = 0; i < c; i++)
		for (int j = 0; j < r; j++) {
			if (j == 0)
				Y[j * c + i] = img[j * c + i];
			else if (j == r - 1)
				Y[j * c + i] = -img[j * c + i];
			else
				Y[j * c + i] = img[(j + 1) * c + i] - img[(j - 1) * c + i];
		}
}
~~~
평소 코딩 습관인데 동적할당 받은 변수에 이미지 픽셀 값을 넣어서 사용한다.

## 2. main 함수 내 변수 선언부
~~~cpp
void main() {
	Mat img = imread("Lenna.png", IMREAD_GRAYSCALE);
	resize(img, img, Size(10, 10));
	int col = img.cols, row = img.rows;

	float* image, * X, * Y;
	image = (float*)malloc(sizeof(float) * col * row);
	X = (float*)malloc(sizeof(float) * col * row);
	Y = (float*)malloc(sizeof(float) * col * row);

	for (int i = 0; i < col; i++)
		for (int j = 0; j < row; j++)
			image[j * col + i] = img.at<uchar>(j, i);
    }
~~~
2가지 설명이 필요할거 같은데

먼저 Gradient를 float로 선언한 이유는,
앞으로 Gradient가 나눗셈, 제곱근 등등 연산과정에서 int형태로는 값 누락이 나올 수 있다.

두번째로 image라는 변수를 선언해 사용하는 이유는 Mat클래스의 at함수의 호출을 최소화 하기 위해서이다.
만약 위의 코드의 image가 있는 부분에 모두 at함수가 사용된다면 변수접근과는 많은 시간차이가 존재할것이다.

ptr은 솔직히 귀찮아서 안씀.

## 3. main 함수 전체
~~~cpp
void main() {
	Mat img = imread("Lenna.png", IMREAD_GRAYSCALE);
	resize(img, img, Size(10, 10));
	int col = img.cols, row = img.rows;

	float* image, * X, * Y;
	image = (float*)malloc(sizeof(float) * col * row);
	X = (float*)malloc(sizeof(float) * col * row);
	Y = (float*)malloc(sizeof(float) * col * row);

	for (int i = 0; i < col; i++)
		for (int j = 0; j < row; j++)
			image[j * col + i] = img.at<uchar>(j, i);
	Grad(image, X, Y, col, row);
  printf("image pixel value >>\n");
	for (int i = 0; i < col; i++) {
		for (int j = 0; j < row; j++) {
			printf("%4d ", (int)image[j * col + i]);
		}
		printf("\n");
	}
	printf("\nimage X gradient value >>\n");
	for (int i = 0; i < col; i++) {
		for (int j = 0; j < row; j++) {
			printf("%4d ", (int)X[j * col + i]);
		}
		printf("\n");
	}
	printf("\nimage Y gradient value >>\n");
	for (int i = 0; i < col; i++) {
		for (int j = 0; j < row; j++) {
			printf("%4d ", (int)Y[j * col + i]);
		}
		printf("\n");
	}

}
~~~

## 4. Result
~~~bash
image pixel value >>
 166  119  116  133  113   88  104  115   73  161
 115  107  104  106  107  115  109  102   65   59
 139  132  195  198  129   97  105   93   57   55
 143  133  144  144   55   58  119   77  143   55
 139  171  179   97   96  117  138   70  138  143
 138  199  195   59  168  162  163  161  154  155
 135  129  168  193  213  136  133   72  201  174
 164  156  178   96   68   61   92  152  152  107
 151  163   62   67  164  156  151  141  217  130
 105   60  165  159  161  164  208  209  118  116

image X gradient value >>
 166  119  116  133  113   88  104  115   73  161
 -27   13   79   65   16    9    1  -22  -16 -106
  28   26   40   38  -52  -57   10  -25   78   -4
   0   39  -16 -101  -33   20   33  -23   81   88
  -5   66   51  -85  113  104   44   84   11  100
  -4  -42  -11   96  117   19   -5    2   63   31
  26  -43  -17   37 -100 -101  -71   -9   -2  -48
  16   34 -106 -126  -49   20   18   69   16  -44
 -59  -96  -13   63   93  103  116   57  -34    9
-105  -60 -165 -159 -161 -164 -208 -209 -118 -116

image Y gradient value >>
 166  -50   14   -3  -45   -9   27  -31   46 -161
 115  -11   -1    3    9    2  -13  -44  -43  -59
 139   56   66  -66 -101  -24   -4  -48  -38  -55
 143    1   11  -89  -86   64   19   24  -22  -55
 139   40  -74  -83   20   42  -47    0   73 -143
 138   57 -140  -27  103   -5   -1   -9   -6 -155
 135   33   64   45  -57  -80  -64   68  102 -174
 164   14  -60 -110  -35   24   91   60  -45 -107
 151  -89  -96  102   89  -13  -15   66  -11 -130
 105   60   99   -4    5   47   45  -90  -93 -116
~~~
보기 깔끔하게 타입 캐스팅을 해서 print해보았다.
이렇게 만든 Gradient 값으로 여러 이미지의 특징들을 찾을 수 있다.
다음에는 Gradient를 어떤 방식으로 활용하는지 포스팅 해볼까 싶기도.
