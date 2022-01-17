---
layout: post
title: Depth estimation from image structure
description: >
    Depth estimation from image structure 논문 리딩
hide_lase_modified: true
categories:
 - paper
---

# Depth estimation from image structure 논문 리딩

## 1. Introduction

<p align="center">
<img src="/assets/img/etc/graduation01.gif"  width="600" height="370">
<br> fig. 1
</p>

그림에서 볼 수 있듯이 단안 이미지(monocular image)에서 절대적인 깊이를 판단하는 것 근본적으로 문제가 존재합니다. 스테레오 이미지, 모션, 디포커스와 관련된 정보가 없다.
위 사진의 fig.1(a)와 같이 3개의 정육면체 사물은 같은 크기로 인식된다.
<br><br>
하지만 이러한 문제는 실제 세계에서 다소 다르게 해석 될 수 있다.
fig.1(b)에서 볼 수 있듯, 자연 구조, 인공 구조는 구조물의 스케일에 따라 달라집니다.

###### _(ex1: 나뭇잎, 나무, 산 등 절대적 크기별로 구조가 다르게 보임)_
###### _(ex2: 빌딩, 자동차, 실내 가구 등 인공 구조물 또한 동일)_
<br>
깊이 정보를 복구(추측)하는 대부분의 기술은 상대 깊이 정보에 초점을 맞춘다.
이러한 상대 깊이 추측 기술은 특정 정보에 의존적이다. (binocular image, motion parallax, defocus)
하지만 일반적인 관측 조건에서 찍은 사진을 보면 관측자들은 절대 깊이에 대한 대략적 추정치를 말 할 수 있다.
<br><br>

__해당 논문에서는 이미지를 영역 또는 물체로 구분지어 분석할 필요없는 monocular image의 절대 깊이 계산을 위한 소스를 도입한다.__

## 2-1. Image Structure

__Discrete Fourier Transform:__
<br>
$$
I(\mathbf{f}) = \Sigma_{x=0}^{N-1}\Sigma_{y=0}^{N-1}i(\mathbf{x})h(\mathbf{x})e^{-j2\pi<\mathbf{f}, \mathbf{x}>}
$$

$$i(\mathbf{x})$$는 쉽게 말해 이미지의 pixel 값이다. 또한 $$j = \sqrt -1$$

공간 주파수 $$\mathbf{f} = (fx, fy) \in [-0.5, 0.5] \times [-0.5, 0.5]$$
다시말해 $$\mathbf{f}$$는 이미지의 pixel 좌표 $$(x, y)$$를 $$(\frac{W / 2 - x}{W}, \frac{H / 2 - y}{H})$$형태로 변환한 값이며
따라서 $$[-0.5, 0.5] \times [-0.5, 0.5]$$의 범위를 가진다는 것을 알 수 있다.
$$h(x)$$는 boundary effect를 줄이기 위한 circular window라고 서술되어 있다. (추가적인 검색 필요)

우리는 이 수식을 통해 결국 이미지의 amplitude spectrum(진폭 스펙트럼)을 구현 해야 한다.<br>
amplitude spectrum은 푸리에 트랜스폼의 크기이다: $$A(\mathbf{f}) = |I(\mathbf{f})|$$

해당 식을 통해 amplitude spectrum을 구했다면, 우리는 amplitude spectrum의 평균 값을 통해 이미지의 스펙트럼 시그니처(S)를 정의 할 수 있다.

<p align="center">
<img src="/assets/img/etc/graduation02.gif"  width="600" height="370">
<br> fig. 2
</p>
fig. 2는 6000개의 이미지로부터 수집한 스펙트럼 시그니처의 50%, 80% contour plot이다.
각각 인공 구조물, 자연 구조물에 대한 plot으로 각각이 다른 특성을 보이고 있다.

인공 구조물의 plot은 수평 및 수직 방향으로 우세한 모양을 갖는다.
자연 구조물의 plot은 가로 세로 방향으로 다소 강조된 전 방향으로 우세한 모양을 갖는다.

## 2-2. Spatial Organization and Local Spectral Signatures

이미지 묘사의 중요한 측면은 global amplitude spectrum이 표현하지 않는, 이미지의 구조적 요소의 공간적 배열에 관한 것이다.
(ex: 파라노마 이미지는 상단에 하늘이 위치하고 낮은 spatial frequency를 갖고, 중심 부분에 수평선, 대개 하단의 texture로 특징된다. )

이러한 구조적 배열은 특정한 패턴 방향과 스케일을 갖는다. 이것은 wavelet transform으로 묘사 될 수 있다.
<br>
$$I(\mathbf{x}, k) = \Sigma_{\mathbf{x'}}i(\mathbf{x'})h_{k}(\mathbf{x} - \mathbf{x'})$$

<br>

어떤 $$h_{k}$$ 함수를 선택하는지에 따라 다른 표현을 제공한다.
가장 일반적으로 사용되는 함수는 Gabor's wavelet :
<br>
$$h_{k}(\mathbf{x}) = e^{-\pi||\mathbf{x}||^{2} / \sigma_{k}^{2}e^{-2\pi j<f_{k}, \mathbf{x}>}}$$
<br>
$$I(\mathbf{x}, k)$$는 $$\mathbf{x}$$에서 $$f_k$$로 정의된 공간 주파수를 complex Gabor filter로 조정한 결과값이다. 이것은 local scale과 orientation information을 encode한 결과이다.

2번째 케이스로 $$h_{k}$$가
<br>
$$h_{k}(\mathbf{x}) = h_{r}(\mathbf{x})e^{-j2\pi<f_{k}, \mathbf{x}>}$$
<br>
이라면 $$I(\mathbf{x}, k)$$는 Windowed Fourier Transform과 일치한다. 그리고 2-1. 에서 쓰여진 $$\mathbf{f}$$를 통해 좀 더 편리하게 정의 할 수 있다.
<br>
$$A(\mathbf{x}, k) = |I(\mathbf{x}, k)|$$는 $$mathbf{x}$$와 인접한 지역의 구조적 정보를 제공한다. Gabor Transform과 WFT모두 비슷한 구조 정보를 제공한다. 시각화의 목적으로 이 논문에서는 WFT를 사용한다.
<p align="center">
<img src="/assets/img/etc/graduation03.gif"  width="600" height="370">
<br> fig. 3
</p>
fig. 3에서는 section별로 10 x 10 window를 사용했다.


## 3. Image Structure as a Depth Cue

이번 섹션에서는 main source인 스펙트럼 특징과 평균 깊이의 variablity에 대해 소개한다.

많은 연구들은 자연 구조물 이미지의 통계에서 불변적 스케일 특성에 집중했다. 이러한 연구들은 대부분 작은 스케일링 변화에 초점을 맞춘다. 그리고 스케일을 위한 절대 단위(ex: meter)을 사용하지 않는다.

그러나, 스케일의 큰 변화를 고려할 때(factor > 10), 이미지의 통계와 절대 단위 중요한 변화가 존재한다.
영상 구조와 이미지 평균 깊이 사이이ㅢ 종속성을 설명할 수 있는 2가지 이유가 있다.

* The point of view

> 정상적인 조건에서 관찰자가 채택한 특정한 장면의 관점은 강제적이다(?). 물체는 대부분의 관점에서 관찰된다. 하지만 거리와 스케일은 사람의 스케일을 앞지른다. 가능한 뷰표인트는 제한되고 예측되게 바뀐다. 결과적으로 이미지의 우세한 방향은 관점, 주된 구조의 공간 배열에 따라 매우 다르다.(지면의 위치, 수평선)

* the building blocks
> 빌딩 블록은 장면을 구성하는 요소를 가리킨다.빌딩 블록은 자연과 인공 환경에서 매우 다르다. 또한 실내와 야외에서도. 장면의 빌딩 블록은 또한 기능적 제약과 각 스케일에서 공간을 만드는 물리적 과정 때문에 큰 차이가 있다.

## 3-1. Relationship between the Global Spectral Signature and Mean Depth
