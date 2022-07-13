---
title: ReLU
tags:
keywords:
summary: 'ReLU(Rectified Linear Unit) 경사함수는 일반적으로 많이 사용하는 활성화 함수 중 하나이다. Sigmoid 와 tanh 의 단점 "Gradient Vanishing" 문제를 해결하기 위한 함수이다.'
sidebar: mydoc_sidebar
permalink: ReLU.html
folder: Basic_Activation_Functions
---

## Introduction

<p align="center">
  <img width="" height="" src="images/ReLU/ReLU_graph.png">
</p>

<p align="center">
  <img width="" height="" src="images/ReLU/ReLU_formula.png">
</p>

x가 0보다 크면 기울기가 1인 직선을 만들고 0보다 작으면 값이 0이 된다. Sigmoid, tanh 함수보다 간단하면서 학습이 빠르고 연산 비용이 적다. 하지만 x의 값이 음수일 경우 여전히 sigmoid 와 tanh 과 같이 뉴런이 죽는 단점이 있다.

## Derivation

### Leaky ReLU

Leaky ReLU는 위에 설명한 ReLU가 갖는 Dying ReLU (뉴런이 죽는 현상)을 개선하기 위해 파생된 함수이다.

<p align="center">
  <img width="" height="" src="images/ReLU/LeakyReLU_graph.png">
</p>

<p align="center">
  <img width="" height="" src="images/ReLU/LeakyReLU_formula.png">
</p>

0.01 또는 다른 매우 작은 값이 적용될 수가 있다. Leaky ReLU는 x가 음수인 영역의 값에 대해 미분값이 0이 되지 않는 점을 제외하면 ReLU와 동일하다.

### PReLU

PReLU는 거의 Leaky ReLU와 유사하다.

<p align="center">
  <img width="" height="" src="images/ReLU/PReLU_graph.png">
</p>

<p align="center">
  <img width="" height="" src="images/ReLU/PReLU_formula.png">
</p>

하지만 새로운 파라미터 a를 추가해 x가 음수인 영역에서도 기울기를 학습한다.

## ELU

Exponential Linear Unit은 ReLU의 모든 장점을 포함하는 것과 동시에 Dying ReLU 문제를 개선했다.

<p align="center">
  <img width="" height="" src="images/ReLU/ELU_graph.png">
</p>

<p align="center">
  <img width="" height="" src="images/ReLU/ELU_formula.png">
</p>

x < 0 일 때 음수의 값이 들어오므로 활성화 함수의 평균 출력이 0에 가깝게 수렴하지만 0은 아니다. 만약 큰 음숫값일 경우 a 값을 통해 ELU가 수렴할 값을 정의한다. 하지만 exp 함수를 계산하는 비용이 발생하기 때문에 추론시간이 느려진다.

## SELU

SELU는 ELU 함수에 scale 값이 추가된 함수이다.

<p align="center">
  <img width="" height="" src="images/ReLU/SELU_graph.png">
</p>

<p align="center">
  <img width="" height="" src="images/ReLU/SELU_formula.png">
</p>

2개의 파라미터를 사용하여 학습을 시키면, 활성화 함수의 분산이 일정하게 나와 성능이 좋아진다고 한다. 하지만 알파 값에 따라 활성화 함수의 결괏값이 일정하지 않아 층을 많이 쌓을 수 없는 단점이 있다.
