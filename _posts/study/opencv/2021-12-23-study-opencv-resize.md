---
layout: post
title: OpenCV Resize함수 직접 구현하기
description: >
    resize 함수 직접 구현
hide_lase_modified: true
categories:
 - study
 - opencv
---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8919104066540378"
     crossorigin="anonymous"></script>
     
## OpenCV Resize함수 직접 구현하기

#### 1. Code
~~~cpp
void myResize(Mat src, Mat dst, int cx, int cy) {
    int i, j, x1, y1, x2, y2;
    double rx, ry, p, q;
    double B_val, G_val, R_val;

    for (j = 0; j < cy; j++)
        for (i = 0; i < cx; i++)
        {
            rx = (src.cols - 1) * i / (cx - 1.);  
            ry = (src.rows - 1) * j / (cy - 1.);

            x1 = rx; // rx보다 작은 최대의 정수
            y1 = ry; // ry보다 작은 최대의 정수

            x2 = x1 + 1; // rx보다 큰 최소의 정수
            if (x2 == src.cols) x2 = src.cols - 1;
            y2 = y1 + 1; // ry보다 큰 최소의 정수
            if (y2 == src.rows) y2 = src.rows - 1;

            p = rx - (double)x1;
            q = ry - (double)y1;

            // 픽셀에 가중치 곱셈 후 해당 픽셀 값 정의
            B_val = (1. - p) * (1. - q) * src.at<Vec3b>(y1, x1)[0]
                + p * (1. - q) * src.at<Vec3b>(y1, x2)[0]
                + (1. - p) * q * src.at<Vec3b>(y2, x1)[0]
                + p * q * src.at<Vec3b>(y2, x2)[0];

            G_val = (1. - p) * (1. - q) * src.at<Vec3b>(y1, x1)[1]
                + p * (1. - q) * src.at<Vec3b>(y1, x2)[1]
                + (1. - p) * q * src.at<Vec3b>(y2, x1)[1]
                + p * q * src.at<Vec3b>(y2, x2)[1];

            R_val = (1. - p) * (1. - q) * src.at<Vec3b>(y1, x1)[2]
                + p * (1. - q) * src.at<Vec3b>(y1, x2)[2]
                + (1. - p) * q * src.at<Vec3b>(y2, x1)[2]
                + p * q * src.at<Vec3b>(y2, x2)[2];

            // Target image에 값 넣기
            dst.at<Vec3b>(j, i)[0] = (limit(B_val + .5));
            dst.at<Vec3b>(j, i)[1] = (limit(G_val + .5));
            dst.at<Vec3b>(j, i)[2] = (limit(R_val + .5));

        }
}
~~~

#### 2. Result
![resize](/assets/img/etc/resize.jpg)
