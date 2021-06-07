---
title: Bayesian Inference
category: Theory
order: 1
use_math: true
comments: true
toc: true
toc_sticky: true
toc_label: 목차
---

### Reference
이 포스팅은 베이지안 통계의 기본적인 내용만 다루기 때문에 수식 도출 과정, 예제, 코드는 오만숙 교수님의 책을 참조하기 바란다.
- R과 JAGS 몬테칼로와 함께하는 베이지안 통계추론(2017), 오만숙
- JAGS를 활용한 베이지안 자료분석(2019), 오만숙
- [MIT 강의자료](https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading20.pdf)

[사전 지식] 확률론

# Contents
1. 기본 개념
2. Frequentist와 Bayesian의 차이
3. 베이지안 추론
4. 마코브체인 몬테카를로 시뮬레이션

3장은 데이터에 설정한 분포와 사전분포에 관한 적분해 사후 분포를 구한다. 적분이 간단한 경우 3장과 같이 사후 분포를 구하지만, 적분이 복잡할 경우 4장과 같이 MCMC 시뮬레이션으로 사후분포에 근사한 샘플을 추출한다.


# 1. 기본개념
- Unit1. 베이즈 공식: 사건에 대한 조건부 확률
$ P(A|B) = \frac{P(A|B)P(A)}{P(B)} $
- Unit2. 베이지안 추론에서의 베이즈 공식 <br/>
$ P(H|D) = \frac{P(D|H)P(H)}{P(D)} $ <br/>
H: hypothesis <br/>
D: data <br/>
$ P(H) $ : Prior probability, 데이터를 고려하기 전 $ H $ 가 참일 때의 확률 <br/>
$ P(H|D) $ : Posterior probability, 데이터를 고려한 후 $ H $ 가 참일 때의 확률 <br/>
$ P(D|H) $: Liklihood, 모든 가능한 hypothesis가 고려될 때 데이터의 발생 확률 <br/>
- 확률공간의 분할(배반인 사건)과 베이즈 정리 <br/>
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
- hypothesis에 대한 확률을 가정하지 않음. no prior, no posterior
- 관측, 비관측 데이터 모두 우도 함수에 의존
- 계산량이 비교적 적음

# 3. 베이지안 추론
## 3.1. 베이지안 추정 

>[요약] <br/>
>사전분포 $ \pi(\theta) $ & 자료 $ f(x|\theta) $
-> 사후분포 $ \pi(\theta|x) $

기본적으로 사후분포를 유도하는 방법은 다음과 같다.
<center>$ \pi(\theta|x_1, ...x_n) = \pi(\theta)f(x_1, ...,x_n|\theta)/f(x_1, ...x_n) $ </center>
<center>$ \propto \pi(\theta)f(x_1, ...,x_n|\theta) $ ...(a) </center>
<center>$ = \pi(\theta)\prod_{i=1}^{n}f(x_i | \theta) $ ...(b)  </center>
$ f(x_1,..., x_n) $ 은 모수 $ \theta $ 와 무관하므로 상수값이기 때문에 (a)가 성립한다.
또한 각 자료가 iid인 경우 (b)와 같이 식이 간단해진다. 식 정리를 한 후 어떤 분포를 따르는지 확인하면 된다.
3.4의 공액 분포에서는 알려진 분포를 따르는 경우를 다룬다.

## 3.2. 베이지안 신뢰 구간

- 베이지안 신뢰 구간: $ 100(1-\alpha) $ % 신뢰구간은 다음을 만족하는 $ \theta $ 의 집합 $ C $ <br/>
$ P(\theta \in C | x ) = 1 - \alpha $
- 최대사후구간(HPD, Highest Posterior Density interval): $ 100(1-\alpha) $ % HPD 구간은 다음을
만족하는 $ \theta_l $ , $ \theta_h $ 사이의 구간 <br/>
1) $ P( \theta_l < \theta < \theta_h ) = 1 - \alpha $ <br/>
2) if $ \theta_a \in (\theta_l, \theta_h) $ and 
$ \theta_b \notin (\theta_l, \theta_h) $ , then $ P(\theta_a|x) > P(\theta_b|x) $

[최대 사후 구간을 찾는 방법] <br/>
사후표본분포가 단봉, 대칭인지 확인하고 HPD 구할 것<br/>
1) 사후분위수 $  (\theta_{\alpha/2},\theta_{1-\alpha/2}) $ 를 대략적인 HPD 구간으로 설정 <br/>
2) 격자를 이용해 신뢰수준을 만족하는 확률밀도 누적합 구간 구함 <br/>
3) 사후분포로부터 표본을 뽑아 신뢰구간 구함(샘플이 사후분포에 근사)

## 3.3. 예측 확률
위에서 구한 사후분포를 이용해 현재 가진 데이터로 $ x_1, ..., x_n $ 부터 
미래 관측치  $ X_{n+1} $ 의 예측 분포를 구할 수 있다.
<center> $ P(X_{n+1}|x_1, ..., x_n) = \int P(X_{n+1}, \theta |x_1, ..., x_n) d \theta $ </center>
<center> $ = \int P(X_{n+1} | \theta,x_1, ..., x_n) \pi(\theta|x_1, ..., x_n) d \theta $ ...(a) </center>
<br/>
joint 분포로부터 적분을 통해 marginal 분포를 구하는 것과 베이지안 정리를 이용하면 (a) 식을 도출할 수 있다.
<center>$ P(a) = \int P(a,\theta) d\theta = \int P(a | \theta) \pi(\theta) d\theta $ </center>
<br/>
위 식의 given condition에 전부 $x_1, ..., x_n $ 을 넣으면 된다.


## 3.4. 공액 분포(Conjugate Distribution)
사전분포와 사후분포의 분포족이 같을 경우 두 분포를 공액분포(conjugate distribution)이라 한다. 
이 경우의 사전분포를 공액 사전분포(conjugate prior)라 한다. <br/>
<br/>


1. 베타분포(사전분포) + 이항분포(데이터) -> 베타분포(사후분포)  <br/>
도출 과정은 [위키피디아](https://en.wikipedia.org/wiki/Beta-binomial_distribution)를 참고한다.  <br/>
데이터: $ x | \theta \sim B(n,\theta) $ <br/>
사전분포: $ \theta \sim Beta(a,b) $  <br/>
사후분포: $ \theta|x \sim Beta(x+a, n-x+b) $ <br/>
 
2. 감마분포(사전분포) + 포아송분포(데이터) -> 감마분포(사후분포)
도출 과정은 다음 [강의자료](https://people.stat.sc.edu/Hitchcock/slides535day5spr2014.pdf)를 참고한다. <br/>
데이터: $ X_1, ...,X_n | \theta \sim Poi(\theta) $ <br/>
사전분포: $ \theta \sim Gamma(a,b) $  <br/>
사후분포: $ \theta|x_1, ...,x_n \sim Gamma(a+ \sum x_i, b+n) $  <br/> 

3. 정규분포 <br/>
도출과정은 다음 [강의자료](https://www.cs.ubc.ca/~murphyk/Papers/bayesGauss.pdf)를 참고한다 <br/>
3.1. 분산이 알려진 경우 <br/>
$ X_1,...,X_n | \theta \sim N(\theta, \sigma^2) $ <br/>
감마분포( $ \theta $ 사전분포) + 정규분포(데이터) -> 감마분포( $ \theta $ 사후분포)  <br/><br/>
3.2. 분산이 알려지지 않은 경우 <br/>
데이터: $ X_1,...,X_n | \theta \sim N(\theta, \sigma^2) $ <br/>
분산 사전분포: $ \sigma^2 \sim IG(a,b) $ <br/>
평균 사전분포: $ \theta|\sigma^2 \sim N(\mu_0, \sigma^2 / k_0) $ <br/>
분산 사후분포: $ \sigma^2|x_1, ...x_n \sim IG(\frac{n}{2} + a, \frac{1}{2} (S + \frac{k_0 n}{k_0 + n}(\bar{x}-\mu_0)^2) +b) $  <br/>
평균 사후분포: $ \theta|x_1, ...x_n, \sigma^2 \sim N(\frac{k_0 \mu_0 + n \bar{x} }{k_0 + n}, \frac{\sigma^2}{k_0 + n})  $ <br/>

4. 다변량 정규분포 <br/>
4.1. 공분산이 알려진 경우 <br/>
$ X = (X_1, ..., X_p) $ <br/>
데이터: $ X \sim N_p(\theta, \Sigma) $ <br/>
평균 사전분포: $ \theta \sim N_p(\mu_0, \Sigma_0) $ <br/>
평균 사후분포: $ \theta | X \sim N_p(\mu_{\pi}, \Sigma_{\pi}) $ <br/>
where <br/>
$ \mu_{\pi} = (n \Sigma^{-1} + \Sigma_0^{-1})^{-1} (n \Sigma^{-1} \bar{x} + \Sigma_0^{-1} \mu_0)$ <br/>
$ \Sigma_{\pi} =  (n \Sigma^{-1} + \Sigma_0^{-1})^{-1} $ <br/><br/>
4.2. 공분산이 알려지지 않은 경우 <br/>
공분산 사전분포인 $ IW $ 는 역위샤트 분포를 의미한다. <br/>
데이터: $ X \sim N_p(\theta, \Sigma) $ <br/>
평균 사전분포: $ \theta | \Sigma \sim N_p(\mu_0, 1/k_0 \Sigma_0) $ <br/>
공분산 사전분포: $ \Sigma \sim IW(\upsilon_0, \Lambda_0) $ <br/>
평균 사후분포: $ \theta|x_1,...x_n, \Sigma \sim N_p( \frac{n \bar{x} +k_0\mu_0}{n+k_0} , \frac{1}{n+k_0} \Sigma ) $ <br/>
공분산 사후분포: $ \Sigma|x_1,...x_n \sim IW(n+\upsilon_0, \Lambda_n) $ <br/>
where <br/>
$ \Lambda_n = \Lambda_0 + S + \frac{n k_0}{n+k_0}(\bar{x}-\mu_0)(\bar{x}-\mu_0)' $ <br/>
$ S = \Sigma_{i=1}{n} (\bar{x}-\mu_0)(\bar{x}-\mu_0)' $
 