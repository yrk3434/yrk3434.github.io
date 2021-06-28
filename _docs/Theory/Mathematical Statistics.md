---
title: Mathematical Statistics
category: Theory
order: 4
use_math: true
comments: true
toc: true
toc_sticky: true
toc_label: 목차
---

- 통계학에서 근본이 되는 몇 가지 성질과 theorem을 다룬다.
- 이 포스팅에서는 확률론, 회귀분석 등 주요 과목과 중복되는 내용은 제외하고, 여러가지 수리적 
성질에 관한 내용만 다룬다.

<br/>
[사전지식] 확률론, 해석학, 미분적분학

## Reference
- [Introduction to Mathematical Statistics, Robert V. Hogg.](https://minerva.it.manchester.ac.uk/~saralees/statbook2.pdf)

# Contents
1. Probabilty and Distributions
2. Multivariate Distributions
3. Some Special Distributions
4. Some Elementary Statistical Inference
5. Consistency and Limiting Distributions
6. Maximum Likelihood Methods
7. Sufficiency
8. Optimal Tests of Hypotheses
9. Inferences About Normal Linear Models
10. Nonparameteric Robust Statistics
11. Bayesian Statistics 

# 1. Probabilty and Distributions
## 1.9. Some Special Expectations

이산확률변수 X의 기대값에 대한 정의는 다음과 같다. 
<center> $ E(X) = \sum_x x p(x) $ </center>

### 적률생성함수
<br/>
분포의 가장 중요한 성질 중 하나는 적률생성함수(mgf, Moment Genrating Function)다. 그 이유는 분포와 mgf는 unique하게 1:1 대응이기 때문이다. 즉, 특정 확률변수의 mgf를 알면 역으로 분포를 알 수 있다.
<br/>  
[적률생성함수 정의]
> <center>$ M(t)=E( e^{tX} )  = \sum_x e^{tX} p(x) $ <center/>

위에서 언급했듯이 분포와 mgf는 1:1 대응이다. 따라서 두 확률변수 $ X $, $ Y $가 동일한 확률분포를 가지면 두 확률변수의 mgf 역시 동일하다.

<center> $ F_X(z) = F_Y(z)$ for all $ z \in \mathbb{R} $ if and only if $ M_X(t) = M_Y(t) $ <center/> <br/>
<center> for all $ t \in (-h,h) $ for some $h>0$  <center/>

<br/>
mgf의 이름처럼 $ M_X(t) $ 를 k차 미분하면 k차 적률을 생성할 수 있다. exponential 함수는 미분꼴이 자기자신과 같기 때문에 mgf의 미분꼴은 단순하다.

<center> $ M'(t) = \frac{M(t)}{dt} = \sum_x xe^{tx}p(x) $ <center/> <br/>
단 연속확률변수의 경우 $ M'(t) = \int_{-\infty}^{\infty} xe^{tx}f(x) dx $ <br/>
여기에 $ t=0 $ 을 대입하면 $ M'(t) = E(X) =\mu $ 다.
  
  
  
