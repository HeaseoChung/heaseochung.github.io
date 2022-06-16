---
title: StyleGAN2
tags: 
keywords:
summary: "StyleGAN V2는 기존의 V1 모델을 향상시킨것이다. CNN 모델의 가중치를 AdaIN 대신 일반적인 정규화를 하고 Progressive Growing 제거해서 부자연스러운 물방울 현상 개선했으며 latent space에 지속성을 제공해서 이미지의 품질을 향상시켰다."
sidebar: mydoc_sidebar
permalink: StyleGAN2.html
folder: Papers
---

## Introduction

StyleGAN은 고해상도의 이미지를 현실적으로 생성한다. 하지만 특정 부분에서 노이즈를 생성하고 또 부자연스러운 부분을 생성하기도 한다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/fa8cd4cd-b240-4ead-ba00-27b5b8229415/StyleGAN_artifacts.png">
</p>

빨간 동그라미로 표시된 노이즈는 항상 발생되는 것은 아니지만 특정 해상도 64x64에 모든 feature maps에서 발생된다. 논문의 저자는 normalization layer (AdaIN) 때문에 생성되는 것이라고 생각하고 있다. 스파이크 유형 분포가 작은 feature map에 들어오면 원래 값이 작더라도 AdaIN에 의해 값이 증가하여 큰 영향을 준다. 

다음은 부자연스러운 부분에 대한 지적이다. 때때로 생성된 이미지의 작은 특성들이 얼굴의 각도가 달라졌음에도 불구하고 고정된 방향성을 가지고 있어서 부자연스럽게 보인다. 이 논문의 저자는 아마 이러한 부자연스러움이 progressive growing에서 비롯된 것이라고 한다. Progressive growing에서 각 해상도에서 이미지를 독립적으로 생성한 것이 문제였던 것이다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/cffbd8ad-0af7-4c69-a356-516ce146d07a/StyleGANv1_artifacts.png">
</p>

StyleGAN2는 이러한 문제점을 해결하기 위해 PPL과 향상된 droplet noise & non-follwing modes로 극복하고자 한다.

<br />

## StyleGAN2
StyleGAN V2는 기존의 V1 모델을 향상시킨것이다. CNN 모델의 가중치를 AdaIN 대신 일반적인 정규화를 하고 Progressive Growing 제거해서 부자연스러운 물방울 현상 개선했으며 latent space에 지속성을 제공해서 이미지의 품질을 향상시켰다. 

*Key Insights*
- AdaIN normalize 대신 estimated statistics of the data(추정 통계)을 normalize을 해서 물방울 현상을 지운다.
- Progressive Growing 대신 skip connection을 갖고 계층 생성자를 이용해서 부자연스러웠던 눈, 이(teeth)의 방향을 제데로 잡는다.
- PPL의 값을 줄이고 latent space 매끄럽게 적용해서 이미지의 품질을 향상시킨다.

<br />

### StyleGAN2's methods

#### Normalization method instread of AdaIN
AdaIN은 statistics of the data(실제 통계)를 input으로 사용해서 정규화를 하는데 이 작업이 물방울 현상(droplet modes) 만든다. 이러한 물방울 현상을 방지하기 위해 저자는 convolution의 가중치를 estimated statistics(추정 통계)을 사용해 정규화 함으로써 물방울 현상을 방지한다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/49ffafe7-f219-4744-bd58-139414f87783/StyleGANV2.png">
</p>

AdaIN은 자세히 설명하면 2개의 스텝으로 나누어질 수 있다. 
1. content information을 정규화
2. style information을 사용해 정규환된 content information을 linear trasition
StyleGAN(a)의 AdaIN부분이 위 처럼 확장된다면 (b)와 같이 된다.

AdaIN의 내부모습
- normalization of content
- linear transformation by style vector

Style block 내부모습
- linear transformation by style vector
- Convolution
- normalization of content

더 나아가서 저자는 평균값으로 작업하는 것은 불필요없는 작업이라고 말한다. 따라서 정규화 작업을 표준 편차로만 나눈다. 또한 스타일의 linear transformation을 계수로 곱한다. 특히 노이즈 삽입 부분은 스타일 블록에서 할 필요가 없기 때문에 스타일 블록에서 꺼냈다(c).

스타일 블록의 내부 작업을 단순화시켜 스타일 백터의 의한 첫 번째 linear transformation은 convolution 안에서 처리가 가능하다. 스타일 블럭에서 linear transformation의 스타일 백터 $$W$$의 계수 $$y_s$$가 사용됐다. Convolution 가중치 $$w_{ijk}$$와 함께 $$s$$를 곱한 content 이미지를 처리하는 작업은 content 이미지 가중치 $$w_{ijk}$$와 $$s$$의 곱과 합성곱 연산하는 것과 같다. 아래의 수식은 간단하게 위의 설명을 표현한 것이다. (연산자 모드: d)

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/e33ee63a-f6a9-4fdd-940e-be5a48eb29e4/Mod_formular.png">
</p>

아래의 수식은 표준 편차의 역수로 output을 곱한다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/3cabcb4a-ce92-45a6-bc4b-c172f04c726c/Mod_formular2.png">
</p>

위의 수식으로, 스타일 블록의 작업 순서는 스타일에 의한 linear transformation -> convolution -> output normalization이 하나의 convolutoin process로 표현될 수 있다. 정규화 부분은 출력이 정규 분포라고 가정한 정규화 과정이다. 이 과정으로 물방울 문제를 야기하는 실제 분포를 사용하는 대신 이 같은 정규 분포를 사용해서 방지한다. 

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/3d9358cb-734d-4638-ad2f-b6fa5daa0ca7/droplet_mode_prevent.png">
</p>

<br />

### A high-resolution image generation method instead of Progressive Growing
기존의 StyleGAN 생성자는 단순한 구조를 가지고 있었다. Progressive Growing 없이 고해상도의 이미지를 만드는 어려움이 있었다. 하지만 생성자와 식별자의 표현 능력 증가로 Progressive Growing없이 고해상도 이미지를 만드는 것이 가능해보였다.

Progressive growing은 점차적으로 생성자와 식별자를 추가해서 고해상도 이미지는 생성하는 가장 보편적인 방식이다. 하지만 각 생성자가 독립적이기 때문에 frequent features를 만들어 사람의 이의 방향성이 항상 고정되어 있는 부자연스러움을 연출시킨다.

그러므로, 저자는 Progressive Growing을 사용하지 않고 고해상도 이미지는 생성하는 모델을 제안한다. 제안한 모델은 MSG-GAN과 비슷하고 아래와 같은 구조를 가지고 있다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/fcc843cf-e86f-4a58-8409-70acd9fc842c/MSG_GAN.png">
</p>

가장 좋은 네트워크를 위해 저자는 (b)와 (c) 모델로 실험을 했다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/c1da166b-5379-4152-ae50-35c1e95ca733/experiment.png">
</p>

그 결과로 위의 표와 같은 결과가 도출되었다. 타입 b는 PPL에서 좋은 성과가 있었고 타입 c는 식별자가 FID를 향상시키는 결과가 나왔다. 저자는 이 모델들을 적용시켜 새로운 모델을 구성했다.

저자는 과연 Progressive Growing 처럼 새로운 모델이 잘 학습할 수 있을지 의문점을 가졌고 아래처럼 데이터를 출력해보았다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/3575e611-abcc-4c6c-a417-6614a85b8145/experiment2.png">
</p>

타입 (b) 생성자는 생성된 이미지의 해상도를 더해, 마지막에 생성된 이미지 해상도의 기여도를 계산한다. 수직축은 각 해상도의 기여도를 설명하고 수직축은 학습 진전도를 설명한다. 새로운 네트워크가 채택된 타입 (a)의 고해상도 영역 학습 기여도는 점차적으로 진전되고 네트워크 크기가 증가되는 반면타입 (b)는 학습시간이 지나면 지날수록 고해상도 영역을 만드는 부분의 기여도가 커진다.

아래의 그림은 기존의 StyleGAN이 생성된 이밎와 새로운 메커니즘을 이용해 생성된 이미지를 비교한 그림이다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/d8ca3b02-fcb6-48a8-8f8f-6e9c4388cb2b/baseline_styleGAN.png">
</p>

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/ed75e7e3-4e5b-48e2-85d0-56772cef9ed9/baseline_styleGANv2.png">
</p>

기존 StyleGAN에서는 눈과 이가 부자연스러운 반면, 새로운 메커니즘은 눈과 이가 얼굴 방향의 변화와 함께 자연스러워졌다.

<br />

### Path Length Regularization to smooth latent space
저자는 latent space의 지각적 매끄러움을 나타내는 PPL과 이미지 품질 사이에 상관관계가 있을 수 있다는 것을 알아냈기 때문에 regularization term으로 통합했다. 수식은 아래와 같다.

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/53e816d9-03b3-49ed-8236-34fd5d51e764/PPL%20regularization.png">
</p>

$$a$$는 상수 값이고 $$y$$는 정규 분포로부터 생성된 랜덤 값이다.

이 정규화는 생성는 생성자가 가능한 latent variable의 미세 변동으로 인한 변화를 최소화 하도록 한다. 학습은 첫 번째 항의 평균을 움직여 상수$$a$$를 동적으로 바꾸면서 최적 값을 설정한다.

<br />

## Distribution of PPL scores

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/ebf943d5-657c-4778-a1c7-d8d6025e146b/results.png">
</p>

위의 그림은 PPL의 분포를 표현한 히스토그램이다. PPL이 작을수록 생성된 이미지의 품질이 더 좋다는 의미이다. (a) 기존 StyleGAN과 (b)의 새로운 StyleGAN2의 히스토그램을 비교했을 때 상대적으로 PPL 분포가 작은 StyleGAN2의 결과 이미지가 더 좋다고 말할수있다.

<br />

## 학습 진행 결과

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/d2da0882-456c-4ca9-a688-90b6391cadef/preds_0.jpg">
</p>

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/f7d60a9b-7847-4fd4-9d1d-66908815204f/preds_1.jpg">
</p>

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/44babf96-1c01-44fb-904d-7f8ef83318ba/preds_10.jpg">
</p>

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/db08ce5e-f2d7-4d40-8108-331f684d0bac/preds_174.jpg">
</p>


<br />

## Github
[StyleGAN2 github by Heaseo Chung](https://github.com/HeaseoChung/StyleGAN2-PyTorch)