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
<figure style="display:block; text-align:center;">
  <img src="/assets/img/etc/graduation01.gif "margin:0px auto">
  <figcaption style="text-align:center; font-size:15px; color:#808080">
    figure 1
  </figcaption>
</figure>
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

## 2. Image Structure

__Discrete Fourier Transform:__
<br>
$$
I(\mathbf{f}) = \Sigma_{x=0}^{N-1}\Sigma_{y=0}^{N-1}i(\mathbf{x})h(\mathbf{x})e^{-j2\pi<\mathbf{f}, \mathbf{x}>}
$$

$$i(\mathbf{x})$$는 쉽게 말해 이미지의 pixel 값이다. 또한 $$j = \sqrt -1$$

공간 주파수 $$\mathbf{f} = (fx, fy) \in [-0.5, 0.5] \times [-0.5, 0.5]$$
다시말해 $$\mathbf{f}$$는 이미지의 pixel 좌표 $$(x, y)$$를 $$(\frac{x - W / 2}{W}, \frac{y - H / 2}{H})$$형태로 변환한 값이며
따라서 $$[-0.5, 0.5] \times [-0.5, 0.5]$$의 범위를 가진다는 것을 알 수 있다.
$$h(x)$$는 boundary effect를 줄이기 위한 circular window라고 서술되어 있다. (추가적인 검색 필요)

우리는 이 수식을 통해 결국 이미지의 amplitude spectrum(진폭 스펙트럼)을 구현 해야 한다.

amplitude spectrum은 푸리에 트랜스폼의 크기이다: $$A(\mathbf{f}) = |I(\mathbf{f})|$$

해당 식을 통해 amplitude spectrum을 구했다면, 우리는 amplitude spectrum의 평균 값을 통해 이미지의 스펙트럼 시그니처(S)를 정의 할 수 있다.
<figure style="display:block; text-align:center;">
  <img src="/assets/img/etc/graduation02.gif "margin:0px auto">
  <figcaption style="text-align:center; font-size:15px; color:#808080">
    figure 2
  </figcaption>
</figure>

fig. 2는 6000개의 이미지로부터 수집한 스펙트럼 시그니처의 50%, 80% contour plot이다.
각각 인공 구조물, 자연 구조물에 대한 plot으로 각각이 다른 특성을 보이고 있다.

인공 구조물의 plot은 수평 및 수직 방향으로 우세한 모양을 갖는다.
자연 구조물의 plot은 가로 세로 방향으로 다소 강조된 전 방향으로 우세한 모양을 갖는다.
