---
title: Distilling the Knowledge in a Neural Network
tags:
keywords:
summary: "가장 기본적으로 머신러닝 알고리즘에서 모델의 성능을 향상시킬 수 있는 방법은 다른 많은 모델들을 같은 데이터로 학습시킨 후 예측 값을 평균으로 구하는 것이다. 하지만 많은 모델을 사용할 수록 연산비용이 증가하여 많은 유저들에 제공하는 것은 어렵다. Caruana 의 연구를 통해 다른 모델들을 한 개의 네트워크 구조로 압축할 수 있다는 것을 증명했고 본 논문는 이것을 기반으로 연구가 되었다. 본 연구는 MNIST를 통해 놀랄만한 결과를 달성했고 무거운 앙상블 모델의 지식을 작은 모델로 이전시켰다. 또한 한개 또는 더 많은 모델들로 구성된 새로운 앙상블 타입을 소개한다. "
sidebar: mydoc_sidebar
permalink: KD.html
folder: Papers
---

## Introduction

실시간으로 추론이 되어야 하는 인공지능 모델의 경우 latency 와 computation complexity 에 민감하다. 특히 cumbersome 모델, 즉 따로 학습 된 모델을 앙상블한 무거운 모델 또는 single network 로 이루어진 무거운 모델은 실시간 서비스에 적용하기가 매우 힘들다. 그렇기 때문에 본 논문은 distillation 으로 무거운 모델이 가지고 있는 지식을 작은 모델에 전달이 가능하다는 것을 증명한 Rich Caruana 의 논문을 기반으로 연구를 시작한다.

일반적으로 학습에 사용된 objective function 은 실제 데이터에서도 반영될 수 있게 해야한다. 그럼에도 불구하고 인공지능 모델들은 보통 트레이닝 데이터셋에 초점을 두어 학습을 하고 최적화를 해서 실제 데이터에서도 좋은 성능을 낼 수 있도록 일반화를 시킨다. 일반화를 잘 하기 위해 모델을 훈련시키는 것이 분명히 더 나을 것이지만, 이를 위해서는 일반화하는 올바른 방법에 대한 정보가 필요하고 일반적으로 이러한 정보를 얻기는 힘들다. 그래서 본 연구는 Knowledge Distiliation (KD) 을 통해 큰 모델의 지식을 작은 모델로 증류하고 큰 모델과 같은 방식으로 일반화하도록 작은 모델을 훈련한다.

큰 모델의 일반화 능력을 작은 모델로 전달하는 확실한 방법은 큰 모델에서 생성된 클래스 확률을 큰 모델을 훈련할 때 soft target 으로 사용하는 것이다. 만약 큰 모델이 대규모 앙상블인 단순한 모델의 경우 예측 분포의 산술 또는 기하 평균을 soft target 으로 사용할 수 있다. Soft target 이 높은 entropy 를 가질 때 하드 타겟보다 훨씬 더 많은 정보를 제공하고 그래디언트 편차가 훨씬 적기 때문에 작은 모델은 종종 원래의 큰 모델보다 훨씬 적은 데이터로 훈련할 수 있다.

MIST 케이스 같은 경우, 큰 모델은 거의 높은 신뢰도 와 함께 정답을 출력한다. 사전에 학습한 큰 모델의 경우 robust 하기 때문에 soft target 에 작은 확률 값을 출력한다. 예로 2, 3, 그리고 7 같은 경우 비슷하게 생겼지만 큰 모델의 경우 확실하게 이 숫자들은 분간한다. 그러므로 확률이 0에 매우 가깝기 때문에 transfer 단계에서 교차 엔트로피 비용 함수에 작은 모델이 거의 영향을 미치지 않는다. Caruna 는 이 문제를 큰 모델이 생성한 logit과 작은 모델의 생성한 logit 간의 제곱 차이를 최소화하는 방법으로 해결한다. Distillation은 큰 모델이 적절하게 soft target 세트를 생성할 때까지 최종 softmax의 temperature 를 올리는 것이다. 그런 다음 이러한 soft target 과 일치하도록 작은 모델을 훈련할 때 동일한 높은 temperature 를 사용한다. 큰 모델의 logit을 일치시키는 것이 목표이다.

작은 모델에 학습할 Transfer 데이터 셋은 라벨이 전혀 되어있지 않아도 된다. 하지만 일반적으로 original 데이터 셋을 사용할 때 KD가 잘되는 모습을 볼 수 있다. 특히, 네트워크가 작은 모델에서는 큰 모델이 주는 soft target 과 매칭하기 위해 더욱 더 original 데이터 셋을 사용하는 것이 좋다.

## Distillation

일반적으로, 이미지 클래스 분류와 같은 task 는 신경망의 마지막 레이어에 softmax 를 추가하여 각 클래스의 확률값을 출력한다.

$$ q_i = \frac {\exp (z_i/T)}{\sum_j \exp(z_j/T)} $$

위의 T 는 temperature 이다. 주로 1로 세팅이 되어있고 1 보다 높은 숫자를 줄 경우 느슨한 클래스 확률값을 출력한다.

가장 간단한 형태의 증류 방식으로, softmax에서 고온의 큰 모델을 사용하여 생성된 transfer set의 각 경우에 대해 soft target distribution을 사용하고 transfer set에 대해 학습하여 지식을 증류된 모델로 전달한다. 학습이 끝난 이후는 temperature 의 셋팅을 다시 1로 변경한다.

<p align="center">
  <img width="" height="" src="images/KD/KD.png">
</p>

$$ Total Loss = 2 \alpha T^2L*{CE}(\sigma \frac {Z_s}{T}, \sigma \frac {Z_s}{T}) + (1-\alpha)L*{CE}(\sigma (Z_S), \hat y)$$

Transfer 세트의 전체 또는 일부에 대해 올바른 라벨이 있는 경우 올바른 라벨을 생성하도록 증류된 모델도 훈련함으로써 크게 모델을 개선할 수 있다. 예로 올바른 라벨을 사용하여 soft target 을 수정하는 것이지만 더 나은 방법은 단순히 두 가지 다른 목적 함수의 가중치 평균을 사용하는 것이다. 첫 번째 목적 함수는 soft target 를 사용하는 cross entropy 이며 이 함수는 큰 모델에서 생성한 soft target 연산하고 증류 모델의 softmax에서 사용한 동일한 high temperature 값으로 연산한다. 두번째의 목적 함수는 Ground Truth 를 사용하는 cross entropy 이다. 하지만 두번째 목적 함수는 high temperature 를 사용하는 것이 아닌 temperature 1을 사용한다. 일반적으로 두번째 목적 함수의 temperature 가 낮은 것이 좋은 결과를 생성한다. alpha 값은 왼쪽항과 오른쪽항에 대한 가중치이다. 예로 Alpha 가 크면 오른쪽항 Loss 의 비중을 두고 학습을 하겠다는 의미이다. T 는 temperature 이고 softmax 함수의 출력값이 큰 것은 아주 크게, 작은 것은 아주 작게 만드는 성질을 완화한다.

## Soft and Hard label

<p align="center">
  <img width="" height="" src="images/KD/Soft_Hard.png">
</p>

위의 그림은 소프트 맥스로 곰, 고양이, 개 3가지 클래스의 확률을 표시한 것이다.
왼쪽 곰, 고양이, 개 같은 경우 0, 1, 0 으로 구성되어 있는 것을 Hard label 오른쪽과 같이 0.05, 0.75, 0.2 으로 구성되어 있는 것을 soft label 이라고 한다. 정리하자면, 클래스 확률의 가중치를 확실히 구분하는 것을 hard 그리고 느슨하게 구분하는 것을 soft 라고 생각하면 쉽다.

## Conclusion

Knowledge distillation 은 사전에 학습시킨 teacher network 의 출력을 실제 서비스에서 사용하고자 하는 작은 모델 Studenet network 가 teacher 를 모방함으로써, 상대적으로 적은 parameter 를 가지고 좋은 성능을 내는 모델을 학습하기 위한 방법론이다.
