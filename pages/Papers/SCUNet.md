---
title: SCUNet
tags:
keywords:
summary: "최근 몇 년 동안 이미지 노이즈 제거를 해결하기 위해 심층 신경망을 활용하는 사례가 급증했지만 기존 방법은 대부분 AWGN, JPEG 압축 및 카메라 센서 노이즈와 같은 단순한 것은 제거할 수 있었고 실제 이미지에서 생기는 블라인드 노이즈 제거에는 역부족이었다. 본 논문에서는 심층 신경망의 아키택처 설계 및 트레이닝 합성 데이터셋의 관점에서 이러한 문제를 해결하고자 한다. 구체적으로 swin-conv 블록을 통해 residual convolutional layer의 local modeling 과 swin transformer 블록을 통한 non-local modeling 기능을 UNet에 통합하는 아키택처를 사용한다. 그리고 합성 트레이닝 데이터셋은 다양한 종류의 노이즈(가우시안, 포아송, 스페클, JPEG 압축 및 처리된 카메라 센서 노이즈)를 포함하고 크기 조정 시 생기는 노이즈를 포함하는 실용적인 LR 데이터셋을 포함한다. 그리고 다양한 노이즈 기법을 순차적으로 적용하는 것이 아닌 Random 하고 double degradation stategy를 적용한다."
sidebar: mydoc_sidebar
permalink: SCUNet.html
folder: Papers
---

## Introduction

Image denosing 이란 noise가 관찰된 y 이미지로 부터 깨끗한 x 이미지를 만드는 프로세스이고 이미지 복원 기법 중 가장 근본적이다. 이 근본적인 문제를 향상하기 위해 많은 연구자들은 2가지 연구 방향성에 초점을 둔다.

1. n이 AWGN 이라는 가정 하에 성능을 개선하는 것
2. 주로 훈련 데이터 또는 노이즈 모델링에 중점을 두는 것

위의 2가지 방법 모두 실제 이미지의 실용성 향상이라는 궁극적인 목표는 같다.

n의 일반적인 가정은 AWGN, JPEG 압축, 푸아송 노이즈 및 카메라 센서 노이즈이며, 이 중 AWGN은 수학성 편의성 때문에 보편적으로 사용되는 노이즈다. 하지만 AWGN을 통해 훈련된 denoise 모델은 실제 이미지에서 생기는 다른 패턴의 노이즈 대해 성능이 좋지 않은 것으로 알려져 있다. 그러므로 본 논문은 Plotz 와 Roth 연구자가 만든 realistic Darmstadt Noise Dataset (DND) 데이터 셋과 카메라 센서 노이즈를 합성한 데이터 셋에서 영감을 받아 실제 이미지에서 생기는 열화를 연구한다. 결과적으로 본 연구는 실제 이미지 열화와 유사한 품질의 데이터 셋을 구축하기 위해 다양한 노이즈(Gaussian, Poisson, speckle, JPEG compression, and processed camera sensor noises), Resizing interpolations(bilinear and bicubic interpolations) 기법을 랜덤하게 적용한다.

<br />

## Method

일반적으로 deep blind model의 경우 깨끗한 이미지와 열화 이미지가 쌍으로 이루어진 데이터 셋을 사용한다. 그리고 여러번의 MAP(Maximum A Posteriori) 문제를 최적화 알고리즘으로 풀어 열화 이미지로 부터 깨끗한 이미지를 얻을 수 있도록 학습을 시킨다. 학습에서 가장 키 포인트는 인공지능이 degradation process 의 패턴을 잘 인지하는 것이다. 일반적으로 패턴을 잘 인지하기 위해 네트워크 아키택처 설계, 모델의 크기 (파라미터의 수), 훈련 데이터에 따라 달라진다.

### Swin-Conv-UNet

<p align="center">
  <img width="" height="" src="images/SCUNet/SCUNET_ARCH.png">
</p>
SCUNet의 메인 아이디어는 DRUNet 과 SwinIR의 네트워크 아키텍처 설계를 통합하는 것이다. 조금 더 정확하게 말하자면 swin-conv(SC) 블럭을 UNet 백본에 연결하는 것이다. 일반적으로 DRUNet은 residual convolution 블럭을 사용하지만 SCUNet는 swin-conv 블럭을 사용해서 DRUNet과 차별점을 두었다. SC block 에는 1x1 conv 2개와 split, concat, SwinT Block, RConv Block이 존재한다. 이미지의 피처가 1x1 convolution 에 통과가 되고 split 을 통해 2개의 피처맵으로 나눠진다. 이후 각 피처맵은 SwinT와 Rconv 블럭으로 통과되고 마지막에 이 둘을 concat으로 합치는 구조이다.

이 구조를 통해 얻는 이점이 있는데 RConv 블럭의 local (특정 영역을 고려) modeling 과 SwinT 블럭의 non-local (전체 영역을 고려) modeling 의 장점을 융합해서 이미즹 특정 영역과 전체 영역을 고려하여 이미지를 복원할 수 있도록 한다. 또한 split & concatenation 연산자가 group convolution 역할을 하기 때문에 computing 복잡성과 파라미터의 수를 줄일 수 있다.

### Training Data Synthesis

<p align="center">
  <img width="" height="" src="images/SCUNet/SCUNET_Data_Synthesis_Process.png">
</p>

본 연구는 실제 열화에서 생기는 LR 데이터를 만들기 위해 double degradation 그리고 random shuffle strategy 를 사용했다.

#### Gaussian noise

대부분 노이즈라고 하면 가우시안 노이즈(AWGN)를 생각하고 사용된다. 본 연구는 기본적인 가우시안 노이즈 대신 R, G, B 채널 간의 노이즈 상관관계를 고려한 3D generalized zero-mean 을 적용한 가우시안 노이즈를 사용해서 카메라 ISP 파이프라인에서 생기는 채널 간의 노이즈 상관관계를 구현했다.

#### Poisson noise

푸아송 노이즈 같은 경우 일반적으로 일정하지 않은 광자의 양 때문에 생기는 노이즈다. 예로 밤에 촬영한 사진, 의학 , 광학 현미경 및 천문학 사진에 많이 보인다.

#### Speckle noise

스펙클 노이즈는 주로 의료용 초음파 이미징과 같은 간섭성 이미징 시스템에서 일반적으로 나타나는 곱셈 노이즈이다. 이 노이즈는 간단히 가우시안 노이즈에 깨끗한 이미지를 곱한 노이즈이다.

#### JPEG compression noise

JPEG 압축 노이즈 같은 경우 사진의 파일 사이즈를 줄일 때 생기는 노이즈이다. 이미지의 파일 사이즈를 0-100% 까지 줄일 수 있다.

#### Processed camera sensor noise

카메라 센서 노이즈 같은 경우 demosaicing, exposure compensation, white balance, XYZ 에서 선형 RGB 색 공간 변환, 톤 매핑 및 감마 보정으로 구성되어 있다.

#### Resizing

리사이징 같은 경우 깨끗한 이미지에 노이즈를 발생시키지 않지만 노이즈가 있는 이미지의 노이즈 분포에 영향을 미친다. 예로 업스케일링은 AWGN이 공간적으로 상관되도록 하는 반면, 다운스케일링은 처리된 카메라 센서 노이즈를 신호 의존성이 덜하도록 한다.

<br />

## Experiments

### 데이터 셋 구성

- DIV2K
- Flick2K

### Loss

- L1 loss

### Optimizer

- Adam optimizer
- Start learning rate: 1e-4
- Decays by a factor: 0.5 every 200,000 iterations

### Patch & Batch size

- Patch size: 128x128
- Batch size: 24

<br />

## Results

<p align="center">
  <img width="" height="" src="images/SCUNet/SCUNET_PerformanceSheet.png">
</p>

<p align="center">
  <img width="" height="" src="images/SCUNet/SCUNET_Model_Size.png">
</p>

<br />

## Github

[SCUnet by Heaseo Chung](https://github.com/HeaseoChung/Super-resolution)

## Reference

[SCUNET 논문](https://arxiv.org/pdf/2203.13278.pdf)
