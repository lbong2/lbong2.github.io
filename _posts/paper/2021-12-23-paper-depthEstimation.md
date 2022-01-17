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

* toc
{:toc .large-only}

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
영상 구조와 이미지 평균 깊이 사이의 종속성을 설명할 수 있는 2가지 이유가 있다.

* The point of view

> 정상적인 조건에서 관찰자가 채택한 특정한 장면의 관점은 강제적이다(?). 물체는 대부분의 관점에서 관찰된다. 하지만 거리와 스케일은 사람의 스케일을 앞지른다. 가능한 뷰표인트는 제한되고 예측되게 바뀐다. 결과적으로 이미지의 우세한 방향은 관점, 주된 구조의 공간 배열에 따라 매우 다르다.(지면의 위치, 수평선)

* the building blocks

> 빌딩 블록은 장면을 구성하는 요소를 가리킨다.빌딩 블록은 자연과 인공 환경에서 매우 다르다. 또한 실내와 야외에서도. 장면의 빌딩 블록은 또한 기능적 제약과 각 스케일에서 공간을 만드는 물리적 과정 때문에 큰 차이가 있다.

## 3-1. Relationship between the Global Spectral Signature and Mean Depth

우리가 작업하고 있는 거리 범위를 위해 스케일링 문제는 하나의 이미지에 대한 확대 출소 배율로 모델링 할 수 없다.(????)

이미지의 크기 및 해상도는 제한되어 있다. 이미지를 2배보다 크게 축소한다면 새로운 구조가 이미지에서 나타나고 그것을 샘플링한다면 작은 디테일이 사라진다.

결과적으로 단순 스케일링 조작으로는 원본이미지와 완전히 다른 구조 모양과 진폭 스펙트럼을 갖게 된다.

다른 깊이에서 장면 구조의 변화를 연구하기 위해 우리는 스펙트럼 시그니처를 이용한다.

인공 구조물과 자연 구조물 사이, 매우 다른 빌딩 블록과 구조와 지역 스케일사이 다른 관계를 보는 것은 매우 흥미롭다.

첫째로 우리는 인공 구조에서 같은 평균 거리$$(D)$$를 공유하는 인공 구조 이미지에서 $$S$$를 정의한다.

<p align="center">
<img src="/assets/img/etc/graduation04.gif"  width="600" height="370">
<br> fig. 4
</p>

fig. 4는 각기 다른 깊이 범위에서 스펙트럼 시그니처를 보여준다. 스펙트럼 시그니처는 [22]의 제안대로
$$A_{D}(\mathbf{f}) ~ \Gamma_{D}(\theta)/||\mathbf{f}||^{\alpha_{D}(\theta)}$$로 모델링된다.

$$\theta = \angle\mathbf{f}$$이고,

$$\Gamma_{D}(\theta)$$는 magnitude preFactor로 방향 function이다.(????)

스펙트럼 시그니처는 선형 (감쇠율?)을 가지고 있습니다. in log 스케일에서

$$-{\alpha_{D}(\theta)}$$에 의해 서술되는 slope가 있다.

여기에 두개의 function은 선형 필터링을 통해 얻어진다. 각각의 스펙트럼의 로그 스케일에서 시그니처 방향에 의해 얻어진다.

<p align="center">
<img src="/assets/img/etc/graduation05.gif"  width="600" height="370">
<br> fig. 5 (a): Man-made (b): natural
</p>

fig. 5는 다른 깊이에서의 평균 slope $$\alpha$$(averaged with respect to orientation)를 보여준다.

평균 slope는 평균 fractal Dimension(프랙탈 차원)을 드러낸다. 이것은 표면 roughness와 clutter와 관련되어 있다.

slope의 증가는 높은 공간 주파수에서 에너지 감소를 의미합니다. 그것은 그러므로 사진의 roughness를 바꾼다.

인공 구조물에서는 우리는 깊이 값이 클 수록단순히 감소하는 slope를 볼 수 있다.(=roughness 증가)

평균적으로, 인공 구조물을 가까이서 볼 때 커다랗고 flat한 표면과 동질적인 영역을 볼 수 있다.(= low roughness)

거리가 증가함에 따라 표면은 부서진 작은 조각처럼 변한다. 그러므로 전체적인 roughness가 증가한다.

---
무엇인가 직감적으로 거리와 함께 노이즈의 증가가 나타나지만, 이것은 일반적인 규칙은 아니며 모든 이미지에 적용되지 않는다.
자연 구조물에서 스펙트럼 시그니처는 평균 깊이와 완전히 대조되는 성격을 드러낸다.

이 사실은 깊이에 따라 roughness가 감소하고 자연 구조물의 확대된 사진에서 표면은 주로 특별한 질감을 가지고 amplitude spectrum에서 작은 감쇠 기울기를 준다.

Depth가 증가하면 자연 구조물은 크고 부드러워진다.

평균적으로 자연 경관 이미지에서는 장면의 깊이가 커질수록 낮은 공간 주파수 + 많은 에너지가 모이는데 이것은 인공 구조물과 반대되는 성격이다.

우세한 방향 또한 의미있는 깊이 관련 정보를 제공한다. 이 점을 보여주기 위해 fig. 5는 인공, 자연 구조물의 여러 깊이에서의 수평, 대각, 수직 방향의 스펙트럼 요소를 보여준다.

ex) 파라노마 뷰: 수평 라인 때문에 스펙트럼 시그니처에서 곧은 수직 모양을 띈다.
ex2) 도심 뷰: 비슷한 양의 수직, 수평방향, 대각 방향으로의 적은 에너지 가짐.

평균적으로 물체의 확대뷰는  강하지 않은 방향 우세성을 가지고 있다. 그러므로 등방의 스펙트럼을 가진다.

## 3-2. Relationship between the Local Spectral Signatures and Depth

fig. 4는 인공, 자연 구조물의 local 스펙트럼 시그니처의 발전(??)을 보여준다.

우리는 깊이에 따라 변하는 local 스펙트럼 시그니처의 일반적인 측면 뿐만 아니라, 뱡향과 척도의 공간 구성을 볼 수 있다.

대표적인 특성은 다음과 같이 요약된다:

* 인공 구조물에서 global roughness의 증가, 자연 구조물에서 감소

* 가까운 거리(depth, D < 10m)에서 스펙트럼 시그니처는 비정상(非正常)적이고 수직, 수평방향으로 편향됨이 없다.

* 중간 정도의 거리(10m ~ 500m)에서는 스펙트럼 시그니처가 비정상(非正常)적이고 수직 수평방향으로의 편향성이 보인다. 평균적으로 장면구조는 아래 부분에 부드러운 표면으로 특징된다. (support surfaces like roads, lakes, fields) 그리고 상단에는 하늘이 있다. 중단부에 인공 구조물은 교차되는 스펙트럼 형태와 높은 주파수 텍스처를 가진 빌딩이 존재하고, 자연 환경은 대부분 전 방향 유사성질을 가진 텍스쳐가 존재한다.

* 먼거리에서는 하늘이 상단 부분에 부드러운 텍스쳐를 진행한다. 하단 부분은 긴 수평면, 작은 네모형태의 인공 구조물 혹은, 부드러운 자연 질감으로 채워진다.

요약하자면 이미지 구조와 평균 깊이 사이의 강한 관련성이 존재한다.

## 4. Low-Dimensional Representations of Image Structure

2.~ 에서 얻었던 영상 특징은 매우 고차원이다. 이번 섹션에서는 그러한 특징들에 이미지 구조의 저차원 표현, 에 대해 논의하고, 고차원 통계에 기반한 다른 구조적 표현에 대해 리뷰한다.

wavelet coefficients 통계에 기반한 다수의 저차원 표현법은 텍스쳐, 물체, 장면 표현에 줄곧 쓰여져 왔다.

global 통계에 기반한 표현법은 텍스쳐를 묘사하는데 편리하다.

전역 통계는 정상성과 전체 이미지의 평균 측정에 의해 계산될 것이라 추정한다.

그것들이 비정상성 이미지를 표현하는데 적절하지 않을지라 전역 통계는 인식을 위한 쓸만한 정보를 제공한다.
<br>
$$A_k^2 = \Sigma_{\mathbf{x}}|I(\mathbf{x}, k)|^2 = \int A(\mathbf{f})^{2}H_{k}(\mathbf{f})d\mathbf{f}$$
<br>
해당 특성$$A_k^2$$은 이미지의 power spectrum이다.
$$A(\mathbf{f})^{2}$$은 푸리에 트랜스폼의 평균이다.
$$H_{k}(\mathbf{f})$$는 wavelet $$h_{k}(\mathbf{x})$$이다.

$$A_{k}$$는 이미지의 2번째 통계 정보를 부호화한다. $$K$$는 분해에 쓰인 wavelet의 개수이다. 그리고, 그러므로, 차원의 표현이다.

이 표현은 우세한 방향 그리고 이미지의 스케일로 부호화 된다.

공간 조직 혹은 이미지 내 물체의 모양에 대한 정보가 없다. 표현에 의해 보존된 정보를 보여주기 위해, fig. 6는 실제 세계의 사진과 같은 $$A_{k}$$값을 가진 사진들을 보여준다. 그러므로 실제 세계 사진과 합성된 텍스쳐는 이 표현에 의해서는 구분되지 않는다.
<p align="center">
<img src="/assets/img/etc/graduation06.gif"  width="600" >
<br> fig. 6
</p>

자연 이미지 통계에 대한 연구들에 따르면 고차 통계량은 실제 사진에서 특정한 형질을 보여준다.

예를 들어 다른 방향과 스케일에서의 wavelet coefficients의 magnitude는 연관된다.

연관성 계수는 이미지 구성과 지역적 분포 요소 함수이다.

The magnitude correlations는
$$A_{i, j}^{2}= \Sigma_{\mathbf{x}}|I(\mathbf{x}, i)||I(\mathbf{x}, j)|$$
로 표현할 수 있다.

이것은 compute에 의해 감소될 수 있지만 해당 표현의 차원은 $$K^{2}$$이다.
