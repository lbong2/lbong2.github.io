---
layout: post
title: OpenCV Rotate함수 직접 구현하기
description: >
    rotate 함수 직접 구현
hide_lase_modified: true
categories:
 - study
 - opencv
---

## OpenCV Rotate함수 직접 구현하기

#### 1. Code
~~~cpp
Mat myRotate(Mat src, double angle) {
    Mat dst;
    dst = Mat::zeros(Size(src.cols, src.rows), CV_8UC3);
    int pt_x = src.cols / 2;
    int pt_y = src.rows / 2;
    int new_x;
    int new_y;
    double pi = 3.141592;
    double seta;

    seta = pi / (180.0 / angle);

    for (int i = 0; i < src.rows; i++) {
        for (int j = 0; j < src.cols; j++) {
            // rotate 행렬을 통해 새로운 좌표 구하기
            new_x = (i - pt_y) * sin(seta) + (j - pt_x) * cos(seta) + pt_x;
            new_y = (i - pt_y) * cos(seta) - (j - pt_x) * sin(seta) + pt_y;

            // 크기를 벗어날 경우 생략
            if (new_x < 0)		continue;
            if (new_x >= src.cols)	continue;
            if (new_y < 0)		continue;
            if (new_y >= src.rows)	continue;

            // Target image에 픽셀 값 정의
            dst.at<Vec3b>(new_y, new_x)[0] = src.at<Vec3b>(i, j)[0];
            dst.at<Vec3b>(new_y, new_x)[1] = src.at<Vec3b>(i, j)[1];
            dst.at<Vec3b>(new_y, new_x)[2] = src.at<Vec3b>(i, j)[2];
        }
    }
    return dst;


}
~~~
#### 2. Result_1
![resize](/assets/img/etc/rotate01.jpg)


위와 같이 빈 공간이 생긴다.
따라서 보간법이 필요하다.
빈 픽셀의 값을 이웃한 픽셀의 평균값으로 채워준다.

#### 3. Interpolate code
~~~cpp
void fnInterpolate(Mat img)
{
    int left_pixval[3] = { 0, };
    int right_pixval[3] = { 0. };


    for (int i = 0; i < img.rows; i++) {
        for (int j = 0; j < img.cols; j++) {
            if (j == 0) {
                right_pixval[0] = img.at<Vec3b>(i, j + 1)[0];
                right_pixval[1] = img.at<Vec3b>(i, j + 1)[1];
                right_pixval[2] = img.at<Vec3b>(i, j + 1)[2];
                left_pixval[0] = right_pixval[0];
                left_pixval[1] = right_pixval[1];
                left_pixval[2] = right_pixval[2];
            }
            else if (j == img.cols - 1) {
                left_pixval[0] = img.at<Vec3b>(i, j - 1)[0];
                left_pixval[1] = img.at<Vec3b>(i, j - 1)[1];
                left_pixval[2] = img.at<Vec3b>(i, j - 1)[2];
                right_pixval[0] = left_pixval[0];
                right_pixval[1] = left_pixval[1];
                right_pixval[2] = left_pixval[2];
            }
            else {
                left_pixval[0] = img.at<Vec3b>(i, j - 1)[0];
                left_pixval[1] = img.at<Vec3b>(i, j - 1)[1];
                left_pixval[2] = img.at<Vec3b>(i, j - 1)[2];
                right_pixval[0] = img.at<Vec3b>(i, j + 1)[0];
                right_pixval[1] = img.at<Vec3b>(i, j + 1)[1];
                right_pixval[2] = img.at<Vec3b>(i, j + 1)[2];
            }


            if ((img.at<Vec3b>(i, j)[0] == 0 && img.at<Vec3b>(i, j)[1] == 0 && img.at<Vec3b>(i, j)[2] == 0) &&
                (left_pixval[0] != 0 || left_pixval[1] != 0 || left_pixval[2] != 0) &&
                (right_pixval[0] != 0 || right_pixval[1] != 0 || right_pixval[2] != 0)) {
                img.at<Vec3b>(i, j)[0] = (left_pixval[0] + right_pixval[0]) / 2;
                img.at<Vec3b>(i, j)[1] = (left_pixval[1] + right_pixval[1]) / 2;
                img.at<Vec3b>(i, j)[2] = (left_pixval[2] + right_pixval[2]) / 2;
            }
        }
    }


}
~~~

#### 4. Result_fin
![resize](/assets/img/etc/rotate02.jpg)
