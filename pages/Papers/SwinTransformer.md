---
title: Swin Transformer
tags: 
keywords:
summary: "본 논문은 새로운 vision transformer, Swin Transformer를 소개한다. 비전 트랜스포머는 이미지가 가지고 있는 다양한 특성과 큰 해상도 때문에 상대적으로 텍스트에서 사용되는 트랜스포머 보다 적용하기 어렵다. 이 문제를 해결하기 위해 연구자들은 shifted windows로 계산되는 hierarchical transformer를 적용했다. Shifted windows를 통해 self-attention에서 non-overlapping local windows를 계산할 때와 동시에 windows들과 연결할 때 발생하는 큰 연산량을 효율적으로 제한할 수 있었다. 더 나아가 hierarchical 구조는 이미지의 여러 특성에 맞게 유동적으로 변화가 가능하고 이미지의 사이즈에 따라 linear 연산을 한다. 결과적으로 swin transformer는 image classification, semantic segmentation 등 다양한 이미지 분야에서 사용할 수 있다."
sidebar: mydoc_sidebar
permalink: SwinTransformer.html
folder: Papers
---

## Introduction
CNN은 컴퓨터 비전 분야에서 주로 사용되던 neural networks 이다. 초기에 AlexNet과 그리고 image classification challenge에서 독보적인 성능을 냈었던 ImageNet 으로 시작해서 현재는 뛰어난 성능으로 다양한 이미지 분야에서 백본으로 사용되고 있다. 반면에 자연어 분야에서는 트랜스포머를 통해 발전을 해왔다. 트랜스 포머는 순차적으로 변환되는 작업을 위해 설계가 되서 데이터에 종속적이다. 자연어 분야에서 성공적인 발전을 이룬 트랜스포머를 비전에도 적용하기 위해 연구자들은 수 많은 연구를 했다.

<br />

본 논문은 트랜스포머가 CNN과 동일하게 일반적인 백본으로 사용할 수 있도록 연구를 시작한다. 자연어 트랜스포머를 비전 트랜스포머로 변환할 때 생기는 2가지의 문제가 있다.

1. 언어 처리에서 기본 요소 역할을 하는 고정된 토큰과 달리 비전에서는 규모가 큰 시각적 요소를 다뤄야하는 문제
2. 이미지는 텍스트 문장보다 큰 차원을 가지고 있어 연산량 문제

<br />

<p align="center">
  <img width="" height="" src="images/SwinTransformer/SwinTransformer_hierarchical_feature_map.png">
</p>

위와 같은 문제를 해결하기 위해 본 연구는 이미지 사이즈에 대해 linear 연산량으로 처리할 수 있는 hierarchical feature map 구조로 설계했다. 이 구조는 작은 사이즈의 패치로 시작해서 깊은 레이어의 인접 패치를 점진적으로 병합한다. 이러한 계층적 특징 맵을 통해 Swin Transformer 모델은 FPN(Feature Parameter Network) 또는 U-Net과 같은 모델에 백본으로 활용할 수 있다. 

<br />

<p align="center">
  <img width="" height="" src="images/SwinTransformer/SwinTransformer_shift_windows_partition.png">
</p>

위의 그림 처럼 Swin Transformer의 핵심 디자인 요소는 연속적인 self-attention layer 사이의 window partition 이동이다. 이동된 window 는 이전 레이어의 window를 연결해 성능을 향상시켰다. 또한 모든 쿼리 패치는 동일한 키 세트 1을 공유하므로 하드웨어의 메모리 접근도 용이하다.

<br />

## Proposed Method

<p align="center">
  <img width="" height="" src="images/SwinTransformer/SwinTransformer_architecture.png">
</p>

위의 네트워크는 Swin transformer tiny 버전이다. 첫째로 input의 RGB를 non-overllaping 패치로 splitting module로 통해 나눈다. 각 패치는 자연어 트랜스포머에서 사용되는 token 처럼 사용된다. 계층 표현을 위해 토큰의 수는 네트워크의 레이어가 깊어짐에 따라 patch merging layer 를 통해 줄어들고 패치의 사이즈는 작아진다. 

<br />

### Swin Transformer block
Swin Transformer는 트랜스포머 블록의 표준 MSA(multi-head self attention) 모듈을 shift-window을 기반으로 하는 모듈로 교체하고 다른 레이어는 동일하게 유지한다. 

<br />

### Self-attention in non-overlapped windows
<p align="center">
  <img width="" height="" src="images/SwinTransformer/SwinTransformer_W-MSA.png">
</p>

효율적으로 연산하기 위해 본 연구는 겹치지 않게 패치로 나누고 local window 안에서 self attention을 하는 방향으로 연구를 했다. 위의 이미지에서 MSA는 이미지의 해상도가 커짐에 따라 quadratic 하게 연산량이 증가하는 반면 W-MSA 는 고정 된 M을 통해 linear 하게 연산할 수 있도록 한다.

<br />

### Shifted window partitioning in successive blocks

하지만 window-based self-attention 모듈의 경우 각 윈도우와 연결되지 못하는 단점이 있다. 본 연구는 이것을 해결하기 위해 a shifted window partitioning approach 를 소개한다. 

<p align="center">
  <img width="" height="" src="images/SwinTransformer/SwinTransformer_shifted_window_partitioning.png">
</p>

Shifted window partitioning approcach 는 window 기반의 multi-head self-attention W-MSA 와 SW-MSA 모듈을 통해 이전 계층의 겹치지 않는 인접 window 사이의 연결을 도와준다.


### Efficient batch computation for shifted configuration

<p align="center">
  <img width="" height="" src="images/SwinTransformer/SwinTransformer_efficient_batch_computation_approach.png">
</p>

Shifted window partitioning 에서 window의 수가 증가할 때 추가 된 window의 크기가 이전에 생성된 것보다 작아 문제를 생길 수 있다. 이 점을 보완하기 위해 위의 그림처럼 efficient batch computatoin approach 을 소개를 한다. 이 방식은 왼쪽 상단 방향으로 순환 이동하는 방식이다. 이동 후 일괄 처리된 창은 feature map에서 인접하지 않은 여러 하위 창으로 구성될 수 있으므로 masking mechanism을 사용하여 각 하위 윈도우 내에서 self attention 연산을 제한한다. Cyclic-shift를 사용하면 배치 윈도우의 수가 일반 윈도우 분할과 동일하게 유지되므로 효율적이다.