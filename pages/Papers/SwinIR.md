---
title: SwinIR
tags:
keywords:
summary: "CNN 은 주로 low-level vision 에서 좋은 성능을 보여줬다. 반면 트랜스포머는 high-level vision 에서 좋은 성능을 보여줬다. 본 논문은 CNN 과 트랜스포머가 갖는 장점을 융합해 이미지 복원 분야에서 사용하는 SwinIR 을 소개한다. SwinIR 은 3가지 파트로 구성되어 있다. 첫번째, 이미지의 low-frequency 를 추출하는 shallow feature extraction. 둘째, residual Swin Transformer blocks 으로 구성되어 있는 high-frequency 를 추출하는 deep feature extration. 마지막으로 high quality image reconstrution 으로 이루어져 있다. 본 논문은 이러한 구성으로 super-resolution, image denoising 그리고 JPEG compression 제거 등에 사용할 수 있는 모델을 연구했고 SOTA 논문들 보다 우수한 성능을 보여줬다."
sidebar: mydoc_sidebar
permalink: SwinIR.html
folder: Papers
---

## Introduction

이미지 복원이란 열화가 많이 생성된 저화질의 이미지에서 깨끗한 고화질의 이미지를 생성하는 것이 주 목적이다. 대부분의 컴퓨터 비전에서 그렇듯 이미지 복원 분야에서도 CNN 이 주로 사용하는 신경밍이였다.
대부분의 CNN 기반의 방법들은 어떻게 하면 깊은 네트워크를 만들 수 있을까 고민을 했고 residual learning 과 dense connections 등의 다양한 방법들이 나왔다. 깊은 네트워크 설계를 통해 성능에 도움이 되었지만, 여전히 2가지 문제에 부딫혔다. 첫째, 고정 된 convolution kernel 을 통해 다양한 특성을 가지고 있는 이미지를 복원하는 것은 좋은 선택이 아니다. 둘째, CNN 은 local processing 만 하기 때문에 이미지의 전체 영역의 다양한 패턴에는 효과적이 못하다.

CNN 의 대체로, 이미지의 전체 영역의 패턴을 인지하는 self-attention 기술이 적용된 비전 트랜스포머가 급부상하고 있다. 하지만 이미지 복원 분야에서는 비전 트랜스포머를 사용하기 어렵다. 비전 트랜스포머는 이미지를 잘라 패치로 만들어 신경망에 통과 시킨다. 이 때, 2가지의 단점이 부각된다. 첫째, borad 픽셀들이 인접한 픽셀들과 연관이 되어 있지 않기 때문에 복원이 쉽지 않다. 둘째, 잘라진 패치 주변에 board artifacts 가 생긴다. Overlapping 을 통해 해결할 수 있지만 연산량 증가로 인해 부정적이다.

최근 Swin Transformer 가 CNN 과 트랜스포머의 장점을 살려 좋은 성능을 보여주고 있다. 특히 CNN 의 장점인 local attention 기술 통해 큰 사이즈의 이미지를 처리하기 좋다. 또한 트랜스포머와 shifted window scheme 을 통해 이미지 전체 영역의 관련성을 학습할 수 있다. 마지막으로 적은 파라미터 수로도 더 좋은 성과를 보여준다.

## Method

### Network Architecture

<p align="center">
  <img width="" height="" src="images/SwinIR/SwinIR_ARCH.png">
</p>

위의 그림을 통해서도 알 수 있지만 SwinIR 은 총 3가지의 모듈로 구성되어 있다.

1. Shallow feature extraction
2. Deep feature extraction
3. High quality image recontruction

#### Shallow and deep feature extraction

Shallow feature extraction 은 3x3 convolution layer 를 통해 이미지의 low-frequency 영역의 특징을 추출하고 학습을 안정적으로 수행할 수 있도록 한다. 그 다음, K 개의 residual Swin Transformer blocks 을 통합하는 Deep feature extraction 모듈을 통해 이미지의 high-frequency 영역을 추춣한다. 마지막으로 3x3 convolution 을 추가하여 추후에 low & high frequency 를 reconstruct 하는 부분에서 좋은 효과를 낼 수 있도록 돕는다.

#### Image reconstruction

Image reconstruction 은 위에서 말한 shallow & deep extraction 모듈에서 추출한 이미지의 feature map 을 합치는 부분이다. Low & high frequency 를 합쳐 학습에 안정성을 보장한다. 그 다음 sub-pixel convolution layer 를 통해 이미지의 해상도를 scale 값에 따라 증가시킨다.

#### Loss function

SwinIR 의 파라미터 최적화하기 위해 L1 Pixel loss 를 최소화하는 것을 목표로 학습을 한다. GT 이미지와 SwinIR 으로부터 통과된 결괏값을 L1 loss 함수에 통과시킨다. 반면에 Real-world image SR 분야에서는 GAN 기반의 손실함수와 perceptual loss 를 추가하였다. 또한, Upsample 이 아닌 단순히 이미지를 제거하는 도메인에서는 Charbonnier loss 를 사용했다.

### Residual Swin Transformer Block

Residual Swin Transformer Block (RSTB) 은 Swin Transformer layers (STL) 과 convolutional layers 로 구성되어 있다.
먼저 RSTB 안에 있는 STL 를 통해 deep feature extraction 을 진행하고 residual connection 을 하기전에 convolutional layer 에 통과 시킨다. 그 결과 equivariance (입력에 따라 변화는 출력의 값) 을 향상시킬 수 있다. 더 나아가, residual connection 을 통해 다른 블록과 연결 지으며 다양한 features 를 reconstruct 모듈에 사용될 수 있다.

#### Swin Transformer layer

<p align="center">
  <img width="" height="" src="images/SwinIR/STL.png">
</p>

Swin Transformer Layer (STL) 은 본래 transformer 에서 사용하는 multi-head self-attention 을 기반으로 만들어졌다. 차이점은 local attention 과 shifted window mechanism 정도이다. $$ H*W*C \longrightarrow \frac {HW}{M^2}*M ^2*C$$
위의 수식처럼 Input 의 사이즈를 M x M 로컬 윈도우 사이즈만큼 나눈다.

$$ Attention(Q,K,V)=SoftMax(QK^T/\sqrt d + B)V $$
$$ Q = Query $$
$$ K = Key $$
$$ V = Value $$
$$ B = learnable \; relative \; position $$

이후 위와 같은 수식으로 로컬 윈도우에서 동시에 self-attention 을 h 번 만큼 병렬적으로 진행하고 결괏값을 multi-head self-attention (MSA) 위해 합친다.

다음은 두 개의 fully-connected layer 와 GELU 이루어진 multi-layer perceptron (MLP) 을 통해 transformation 을 진행한다.

$$ X = MSA(LN(X)) + X $$
$$ X = MLP(LN(X)) + X $$

MSA 와 MLP 를 수행하기 이전에 LayerNorm (LN) 하고 이후에는 residual connection 도 추가된다.

Shifted window mechanism 같은 경우 partition 의 사이즈가 고정일 때, local window 들간에 connection 이 없어 정보 교환이 어려울 때 사용된다. 예로 feature 를 나누기 전에 M/2 만큼 픽셀을 shifting 시켜 사용한다.
