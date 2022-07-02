---
title: BSRGAN
tags:
keywords:
summary: "Single Image Super-resolutoin (SISR)으로 현실에서 훼손된 이미지를 복원할려고 하면 잘 되지 않는 경우가 많다. 여러 종류의 열화기법을 사용해서 SISR 모델을 학습을 시켜도 현실에서 여러 이유로 훼손된 이미지를 복구하기란 쉽지않다. 이 문제를 해결하기 위해, 이 논문은 blur, downsampling 그리고 noise 열화기법을 임의적으로 섞어서 학습 된 모델을 소개한다. 특히 blur 같은 경우 isotropic과 anisotropic 2가지의 가우시안 커널을 사용했고, downsampling은 nearest, blilnear 그리고 bicubic 보간기법을 임의적으로 선택해 사용했다. 게다가 노이지 생성을 위해 가우시안 노이즈, JPEG 압축 노이즈 그리고 camera image signal processing (ISP) 파이프라인 모델을 사용했다. 제안한 모델의 효과성을 입증하기 위해 저자는 deep blind ESRGAN super-resolver를 이용하여 위의 여러가지 열화기법을 포함해서 모델을 학습시켰다. "
sidebar: mydoc_sidebar
permalink: BSRGAN.html
folder: Papers
---

## Introduction

Single image super-resolution (SISR)은 저해상도 이미지 y로 부터 자연스럽고 선명한 고해상도 x를 만드는 것이 목표다. DNNs이 나오면서 빠르고 효과가 좋은 SISR 모델을 만들 수 있었다. 기본적으로 SISR 방법은 열화된 저해상도 이미지를 고해상도 이미지에 매핑을 시켜 학습을 시킨다. 이때 대표적으로 2가지의 열화 방식이 있는데, bicubic 열화와 전통적인 열화 방식이 있다.

<!-- $$y = (x⊗k)↓s+n$$
    - $$(x⊗k)$$는 blurry image
    - $$↓s$$ 는 scale factor s로 downsampling
    - $$n$$ 은 가우시안 노이즈 -->

일반적으로 degradation 모델은 blur kernel 그리고 noise level에 따라 정해진다. 이러한 요인이 사전에 알려져 있는지 여부에 따라 DNN 기반 SISR 방법은 non-blind와 blind 방법으로 나눌 수 있다.

<br />

초기의 non-blind SISR은 주로 bicubic degradation으로 설계되었다. Bicubic의 경우 PSNR와 percetual 부분에서는 상당한 효과가 있었지만 진짜 열화가 된 이미지에는 큰 효과가 없었다. 이러한 이유로 blur kernels이 SISR에서 가장 중요한 역할이라고 생각할 수 있다. Blur kernel과 가우시안 노이즈를 이용한 Non-blind degradation model 같은 경우 bicubic 보다 뛰어난 성능을 보여줬지만, 정확한 kernel정보와 노이즈 정보가 없으면 성능이 많이 안좋았다. 반면에 blind degradation model 같은 경우 kernel을 계산하는 기능과 함께 쓰였지만 정확한 blur kernel과 noise를 계산하기는 역부족이여서 좋은 결과를 얻기는 힘들었다.

<br />

이 논문의 저자는 위의 논의에서 두 가지의 결론을 도출하게 된다.

1. 열화 모델은 DNNs 기반 SISR 방법에 중요하고 현실에서 생기는 열화 모델을 연구할 가치가 있다.
2. 기존의 blind SISR 모델도 현실에서 생기는 열화 이미지에 SISR을 쉽게 적용할 수 없었다.

그리고 두 가지 주요 챌린지를 언급한다.

1. 현실 열화 이미지를 복원하기 위해 실용적인 SISR 열화 모델 설계 \
2. 대부분의 실제 이미지에 잘 맞은 효과적인 deep blind 모델 학습

위에 언급한 열화 모델에 대한 첫번째 챌린지에서 blur, downsampling 그리고 noise가 실제 이미지 열화의 원인이 되는 주요 요인이라고 생각한다.

<br />

### Degradation 모델에 사용한 열화 기법 정리

<p align="center">
  <img width="" height="" src="https://images.velog.io/images/heaseo/post/86e209c5-5284-4c39-a08f-f0117c682c31/BSRGAN_degradation.png">
</p>

#### Blur

Blur는 이미지를 열화시키는 가장 기본적인 방법 중 하나이다. Blur는 downsampling 이후 aliasing과 공간적 정보를 보존하기 위해 사용된다. 이 논문의 저자는 두 가지의 blur 타입을 정의했는데, isotropic gaussian kernel과 anisotropic gaussian kernel 이다. 이를 통해 blur의 열화 공간을 크게 확장할 수 있다.

**Isotropic Gaussian kernel**

- blur kernel size: 7x7, 9x9, ..., 21x21
- sigma uniform: [0.1, 2.8]

**Anisotropic Gaussian kernel**

- blur kernel size: 7x7, 9x9, ..., 21x21
- x_sigma uniform: [0.5, 8]
- y_sigma uniform: [0.5, 8]
- rotation: [0, 180]

<br />

#### Downsampling

고해상도 이미지를 저해상도 이미지를 만들기 위해 가장 쉬운 방법은 nearest neighbor 보간법. 하지만 nearest 보간법을 사용해서 LR 이미지를 만들게 되면 왼쪽 상단 모서리에 불량 픽셀들이 생긴다. 해결책으로 2D 선형 그리드 보간법을 통해 중심을 잡아 픽셀을 이동시켰다. 추가적으로 bicubic과 bilinear 보간법도 추가해서 저해상도 이미지를 만들었다. 더 나아가 단순히 이미지를 downsample을 하는 것이 아니라 다시 보간법으로 upsample도 추가해서 저해상도 이미지를 생성했다.

1. **Bicubic**
2. **Bilinear**
3. **Nearest**

<br />

#### Noise

1. **AWGN with different noise levels**
   - 가우시안 노이즈는 가장 보편적으로 사용하는 노이즈의 종류이다.
   - Noise level: [1,25]
2. **JPEG compression noise**
   - JPEG 확장자를 사용해서 압축을 할 때 생기는 노이즈다.
   - Compression level: [30, 95]
3. **Reverse-forward camera image signal processing (ISP)**
   - 디지털 카메라는 결과 이미지를 받기 위해 raw sensor data를 image signal processing (ISP) pipeline에 통과시키는데 이때 생기는 노이즈다.

<br />

#### Random Shuffle

전통적인 열화기법 모델은 현실의 저해상도 이미지를 복구하기에는 무리가 있었다. 현실 저해상도의 이미지는 노이즈, 블러리, 축소, 그리고 JPEG 압축 노이즈로 복잡하게 훼손이 되어있다. 반면에 전통적인 열화기법 모델은 단순히 bicubic으로 저해상도를 만들어 고해상도 이미지와 지도학습을 시켰기때문에 효과는 미미했다. 그래서 논문의 저자는 현실에 저해상도 이미지의 복잡한 열화를 최대한 비슷하게 구현하기 위해 random shuffle 이라는 방법을 계획한다.

{$$B_{iso}$$, $$B_{aniso}$$, $$D^s$$, $$N_G$$, $$N_{JPEG}$$, $$N_S$$} 을 임의적으로 섞어 열화된 저해상도 이미지를 생성한다.

- $$B_{iso}$$는 iso blur
- $$B_{aniso}$$는 aniso blur
- $$D^s$$는 scale factor of dowmsample
- $$N_G$$는 gaussian noise
- $$N_{JPEG}$$는 JPEG noise
- $$N_S$$는 ISP noise

<br>

## Contributions

- 실제 열화 이미지를 효과적으로 복원할 수 있게 blur, downsampling 그리고 noise를 임의적으로 섞어서 만든 열화 모델을 디자인했다.
- 위의 설계한 열화모델로 학습 데이터를 생성하고 blind SISR 모델을 학습시켜 뛰어난 복원 능력을 보여준다.
- 최초의 blind model을 위한 새로운 열화 모델 기법
- DNNs 기반 SISR 방법의 실제 적용에 대한 정확한 열화 모델링의 중요성을 강조

<br />

<!-- ## Results
<table>
    <tr>
        <td><center>Original</center></td>
        <td><center>BSRGAN x4</center></td>
    </tr>
    <tr>
    	<td>
    		<center><img src="https://images.velog.io/images/heaseo/post/6f2fa011-343a-4e27-ae86-000b44a24727/chip.png"></center>
    	</td>
    	<td>
    		<center><img src="https://images.velog.io/images/heaseo/post/a14905de-8a40-4cb6-aed1-a1eb9612c744/chip_BSRGAN.png"></center>
    	</td>
    </tr>
    <tr>
        <td><center>Original</center></td>
        <td><center>BSRGAN x4</center></td>
    </tr>
    <tr>
    	<td>
    		<center><img src="https://images.velog.io/images/heaseo/post/77efe499-80b6-4c27-941c-c6571f07b8f1/oldphoto2.png"></center>
    	</td>
    	<td>
    		<center><img src="https://images.velog.io/images/heaseo/post/87eff938-198f-4be7-ad04-96ad1059700e/oldphoto2_BSRGAN.png"></center>
    	</td>
    </tr>
</table> -->

<br />

## Github

[BSRGAN by Heaseo Chung](https://github.com/HeaseoChung/Super-resolution)
