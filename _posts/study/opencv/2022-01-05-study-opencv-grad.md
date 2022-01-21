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
	Grad(image, X, Y, col, row);
	printf("image pixel value >>\n");
	for (int j = 0; j < row; j++) {
		for (int i = 0; i < col; i++) {
			printf("%4d ", (int)image[j * col + i]);
		}
		printf("\n");
	}
	printf("\nimage X gradient value >>\n");
	for (int j = 0; j < row; j++) {
		for (int i = 0; i < col; i++){
			printf("%4d ", (int)X[j * col + i]);
		}
		printf("\n");
	}
	printf("\nimage Y gradient value >>\n");
	for (int j = 0; j < row; j++){
		for (int i = 0; i < col; i++)
		{
			printf("%4d ", (int)Y[j * col + i]);
		}
		printf("\n");
	}

}
~~~

## 4. Result
~~~bash
image pixel value >>
 166  115  139  143  139  138  135  164  151  105
 119  107  132  133  171  199  129  156  163   60
 116  104  195  144  179  195  168  178   62  165
 133  106  198  144   97   59  193   96   67  159
 113  107  129   55   96  168  213   68  164  161
  88  115   97   58  117  162  136   61  156  164
 104  109  105  119  138  163  133   92  151  208
 115  102   93   77   70  161   72  152  141  209
  73   65   57  143  138  154  201  152  217  118
 161   59   55   55  143  155  174  107  130  116

image X gradient value >>
 166  -27   28    0   -5   -4   26   16  -59 -105
 119   13   26   39   66  -42  -43   34  -96  -60
 116   79   40  -16   51  -11  -17 -106  -13 -165
 133   65   38 -101  -85   96   37 -126   63 -159
 113   16  -52  -33  113  117 -100  -49   93 -161
  88    9  -57   20  104   19 -101   20  103 -164
 104    1   10   33   44   -5  -71   18  116 -208
 115  -22  -25  -23   84    2   -9   69   57 -209
  73  -16   78   81   11   63   -2   16  -34 -118
 161 -106   -4   88  100   31  -48  -44    9 -116

image Y gradient value >>
 166  115  139  143  139  138  135  164  151  105
 -50  -11   56    1   40   57   33   14  -89   60
  14   -1   66   11  -74 -140   64  -60  -96   99
  -3    3  -66  -89  -83  -27   45 -110  102   -4
 -45    9 -101  -86   20  103  -57  -35   89    5
  -9    2  -24   64   42   -5  -80   24  -13   47
  27  -13   -4   19  -47   -1  -64   91  -15   45
 -31  -44  -48   24    0   -9   68   60   66  -90
  46  -43  -38  -22   73   -6  102  -45  -11  -93
-161  -59  -55  -55 -143 -155 -174 -107 -130 -116
~~~
보기 깔끔하게 타입 캐스팅을 해서 print해보았다.
이렇게 만든 Gradient 값으로 여러 이미지의 특징들을 찾을 수 있다.
다음에는 Gradient를 어떤 방식으로 활용하는지 포스팅 해볼까 싶기도.
