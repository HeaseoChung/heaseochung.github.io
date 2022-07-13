---
title: Sigmoid
tags:
keywords:
summary: "Sigmoid 함수는 Logistic 함수라고 불린다. x의 값에 따라 0에서 1사이의 값을 출력하는 S 모양을 가지는 함수이다."
sidebar: mydoc_sidebar
permalink: Sigmoid.html
folder: Basic_Activation_Functions
---

## Introduction

<p align="center">
  <img width="" height="" src="images/Sigmoid/Sigmoid_formula.png">
</p>

<p align="center">
  <img width="" height="" src="images/Sigmoid/Sigmoid_graph.png">
</p>

Sigmoid 함수는 음수 값을 0에 가깝게 표현하기 때문에 깊어지는 레이어에서 미치는 영향이 적어지는 Vanishing Gradient 문제가 발생한다. 즉 x의 절대값이 커질수록 Gradient Backpropagaton 시 미분 값이 소실될 가능성이 크다.

하지만 모든 실수 값을 0보다 크고 1보다 작은 미분이 가능한 수로 변환하기 때문에 Logistic Classification과 같은 분류 문제의 가설과 비용 함수에 사용된다.
