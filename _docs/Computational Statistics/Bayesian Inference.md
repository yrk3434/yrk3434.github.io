---
title: Bayesian Inference
category: Computational Statistics
order: 1
use_math: true
comments: true
toc: true
toc_sticky: true
toc_label: 목차
---

# Reference
이 포스팅은 베이지안 통계의 기본적인 내용만 다루기 때문에 실질적인 예제와 코드는 오만숙 교수님의 책을 참
조하기 바란다.
- R과 JAGS 몬테칼로와 함께하는 베이지안 통계추론(2017), 오만숙
- JAGS를 활용한 베이지안 자료분석(2019), 오만숙
- [MIT 강의자료](https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading20.pdf)

# Contents
1. 기본 개념
2. Frequentist와 Bayesian의 차이
3. 베이지안 추론
4. 마코브체인 몬테카를로 시뮬레이션

# 1. 기본개념
- Unit1. 베이즈 공식: 사건에 대한 조건부 확률
$ P(A|B) = \frac{P(A|B)P(A)}{P(B)} $
- Unit2. 베이지안 추론에서의 베이즈 공식
$ P(H|D) = \frac{P(D|H)P(H)}{P(D)} $
H: hypothesis
D: data
$ P(H) $ : Prior probability, 데이터를 고려하기 전 $ H $ 가 참일 때의 확률
$ P(H|D) $ : Posterior probability, 데이터를 고려한 후 $ H $ 가 참일 때의 확률
$ P(D|H) $: Liklihood, 모든 가능한 hypothesis가 고려될 때 데이터의 발생 확률
- 확률공간의 분할(배반인 사건)과 베이즈 정리
$ {A_1, A_2,..,A_k} $
$ P(A_i|B) = \frac{P_I(A_i \cap B)}{
,
} $

# 2. Frequentist vs Bayesian
통계학에는 크게 두 학파가 있다. Frequentist와 Bayesian이다. 두 학파의 가장 큰 차이는 사건(확률에서의 사
건)이 발생할 수 있는 공간에 대해 어떤 믿음(확률 분포)을 가정하는지의 여부다. 엄밀한 설명은 아니지만, 위에서
계속 언급되는 hypothesis는 주어진 데이터의 확률 분포가 발생할 수 있는 환경에 대한 가정을 의미한다.
<br/>
<br/>
[Bayesian inference]
- hypothesis와 data에 확률 사용
- prior와 데이터의 우도함수에 영향을 받음
- 주관적인 prior
- parameter를 구하기 위한 적분이 많아 계산량 많음

[Frequentist inference]
- hypothesis에 대한 확률을 가정하지 않음, no prior, no posterior
- 관측, 비관측 데이터 모두 우도 함수 $ P(D | H) $ 에 의존
- 계산량이 비교적 적음

# 3. 베이지안 추정과 베이지안 신뢰 구간
- 베이지안 추정
사전분포 $ \pi(\theta) $ & 자료 $ f(x|\theta) $
-> 사후분포 $ \pi(\theta|x) $
- 베이지안 신뢰 구간: $ 100(1-\alpha) $ % 신뢰구간은 다음을 만족하는 $ \theta $ 의 집합 $ C $ <br/>
$ P(\theta \in C | x ) = 1 - \alpha $
- 최대사후구간(HPD, Highest Posterior Density interval): $ 100(1-\alpha) $ % HPD 구간은 다음을
만족하는 $ \theta_l $ , $ \theta_h $ 사이의 구간 <br/>
$ P( \theta_l < \theta < \theta_h ) = 1 - \alpha $
