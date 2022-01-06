---
layout: post
title: image Histogram of Gradient
description: >
    Histogram of Gradient
hide_lase_modified: true
categories:
 - study
 - opencv
---

# Image Histogram of Gradient

## 1. 용어 정의

#### 1-1. Histogram
```
히스토그램(histogram)은 표로 되어 있는 도수 분포를 정보 그림으로 나타낸 것이다. 더 간단하게 말하면, 도
수분포표를 그래프로 나타낸 것이다. 보통 히스토그램에서는 가로축이 계급, 세로축이 도수를 뜻하는데, 때때로
반대로 그리기도 한다. 계급은 보통 변수의 구간이고, 서로 겹치지 않는다. 그림에서 계급(막대기)끼리는 서로
붙어 있어야 한다. 히스토그램은 일반 막대그래프와는 다르다. 막대그래프는 계급 즉 가로를 생각하지 않고
세로의 높이로만 나타내지만 히스토그램은 가로와 세로를 함께 생각해야 한다. (출처: 위키 백과)
```

#### 1-2. Gradient(기울기)
```
기울기(gradient 그레이디언트[*])란 벡터 미적분학에서 스칼라장의 최대의 증가율을 나타내는 벡터장을 뜻한다.

기울기를 나타내는 벡터장을 화살표로 표시할 때 화살표의 방향은 증가율이 최대가 되는 방향이며,
화살표의 크기는 증가율이 최대일 때의 증가율의 크기를 나타낸다. (출처: 위키백과)
```

위의 2개의 용어가 간단해 보이지만 HOG의 모든 뜻을 담고있는 단어다.

간단하게 미적분학에서 2차함수 그래프의 한 점에서의 기울기는 y증가량/x증가량의 순간 변화율이다.
따라서 이미지에서 Gradient 또한 픽셀 사이의 y증가량/x증가량이라고 볼 수 있고 우리는
특정 이미지의 모든 픽셀에서 Gradient를 구할 수 있다.

#### 2. Image Gradient 구하기
* __void main() 내부__
~~~cpp
int* X, *Y;

X = (int*)calloc(img.rows * img.cols, sizeof(int));
Y = (int*)calloc(img.rows * img.cols, sizeof(int));
~~~
해당 부분은 본인의 코딩 스타일이다. img의 x방향, y방향 Gradient를 X, Y 각각의 동적할당 변수에 저장한다.


* __grad function__
~~~cpp
/// <summary>
/// Gradient 구하는 함수
/// </summary>
/// <param name="in">: 입력 이미지(GrayScale) </param>
/// <param name="r">: 입력 이미지 세로 길이 </param>
/// <param name="c">: 입력 이미지 가로 길이 </param>
/// <param name="X">: X방향 Gradient 저장 </param>
/// <param name="Y">: Y방향 Gradient 저장 </param>
void Grad(Mat in, int r, int c, int* X, int* Y) {
	for (int i = 0; i < c; i++) {
		for (int j = 0; j < r; j++) {
			if (i == 0) {
				X[j * c + i] = in.at<uchar>(j, i + 1);
			}
			else if (i == c - 1)
			{
				X[j * c + i] = -(in.at<uchar>(j, i - 1));
			}
			else
				X[j * c + i] = in.at<uchar>(j, i + 1) - in.at<uchar>(j, i - 1);
		}
	}
	for (int i = 0; i < c; i++) {
		for (int j = 0; j < r; j++) {
			if (j == 0) {
				Y[j * c + i] = -(in.at<uchar>(j + 1, i));
			}
			else if (j == r - 1)
			{
				Y[j * c + i] = in.at<uchar>(j - 1, i);
			}
			else
				Y[j * c + i] = in.at<uchar>(j - 1, i) - in.at<uchar>(j + 1, i);
		}
	}
}
~~~
동적 할당 받은 포인터를 매개변수로 각 방향 Gradient를 구한다.

#### 추후 추가 예정
