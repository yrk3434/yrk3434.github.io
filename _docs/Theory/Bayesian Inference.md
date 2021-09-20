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
- [IOWA 강의자료](https://myweb.uiowa.edu/pbreheny/uk/teaching/701/notes/2-28.pdf)
- Computational Statistics, Wiley Series
- Wikipedia

[사전 지식] 확률론

# Contents
[1. 기본 개념](#1.-기본-개념) <br/>
[2. Frequentist와 Bayesian의 차이](#2.-Frequentist와-Bayesian의-차이) <br/>
[3. 베이지안 추론](#3.-베이지안-추론) <br/>
[4. 계층 모형](#4.-계층-모형) <br/>
[5. 베이지안 회귀](#5.-베이지안-회귀) <br/>
[6. 마코브체인 몬테카를로 시뮬레이션](#6.-마코브체인-몬테카를로-시뮬레이션) <br/>
[7. JAGS를 통한 시뮬레이션 연습](#7.-JAGS를-통한-시뮬레이션-연습) <br/>

3장은 데이터에 설정한 분포와 사전분포에 관한 적분해 사후 분포를 구한다. 적분이 간단한 경우 3장과 같이 사후 분포를 구하지만, 적분이 복잡할 경우 4장과 같이 MCMC 시뮬레이션으로 사후분포에 근사한 샘플을 추출한다.


# 1. 기본 개념
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

# 2. Frequentist와 Bayesian의 차이
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
 
## 3.5. 사전분포
공액분포의 경우 설정할 사전분포의 종류가 정해져있지만 공액분포를 사용하지 않을 경우 스스로 사전분포를 설정해야한다.
사전분포를 가정할 모수공간과 모수의 종류(이산 혹은 연속)를 잘 생각해야 한다.
예를 들어 포아송 분포 모수는 $ \lambda > 0 $ 의 범위를 가지며 연속 분포로부터 나온 값이다.
0보다 큰 연속값을 갖는 분포는 감마, 역감마 분포가 있다.

- 무정보 사전분포: 균등분포 $ U(a,b) $, 사전정보가 유의하지 않을 때 사용
- 변환불변 사전분포: 위치변환에 불변( $ \pi_1(\theta)= \pi_2(\theta+c) $ )또는 
척도변환에 대해 불변( $ \pi_1(\sigma)=  \frac{1}{c} \pi_2(\frac{\sigma}{c}) $ )인 사전 분포
- 제프리 사전분포: $ \pi(\theta) = \sqrt{I(\theta)} $ , 여기서 $I(\theta)$는 피셔의 정보상수 <br/>
하나의 관측치에 대한 피셔의 정보 상수 $ I_1(\theta) $,
iid 관측치의 경우 $ X1,...,Xn $ 의 정보상수는 $ nI_1(\theta) $ 
<center> $ I_1(\theta) = E^{X|\theta}[(\frac{d log f(X|\theta) }{d \theta})^2] $ </center>
<center> $ = - E^{X|\theta}[\frac{d^2 log f(X|\theta) }{ d \theta^2 }] $ </center>

- 공액 사전분포
- 부적합 사전분포: 모수공간을 제대로 반영하지 못하거나 모수공간 적분값이 무한인 경우. 
그러나 사후 분포는 적합할 수 있는데 이 경우는 문제 없음

# 4. 계층 모형
동질적이지 않은 다른 집단으로부터 자료가 수집된 경우 계층 모형을 사용한다.
예컨대, 학생들의 성적을 추출할 때 지역별로 다른 분포를 가진다. 
성적이라는 공통의 특성에 대해 집단 별로 조금씩 다른 분포를 가질 수 있다.
<center>$ X_{ij}|\theta, \sigma^2 \sim N(\theta_i, \sigma^2) $ </center>
<center> where i: group, j: observation </center>
<center>$ \theta_i|\mu, \gamma^2 \sim N(\mu, \gamma^2) $ </center>
<center>$ \sigma \sim IG(a,b) $ </center>
<center>$ \mu|\gamma^2 \sim N(\mu_0, \gamma^2/k_0) $ </center>
<center>$ \gamma^2 \sim IG(c,d) $ </center>
위 모델에 대한 사후분포 도출은 [강의자료](http://www.stat.cmu.edu/~brian/463-663/week10/Chapter%2009.pdf)
의 p11~p15(9.2.1 Random effects: The random intercept model)을 참고한다.


# 6. 마코브체인 몬테카를로 시뮬레이션

사후확률을 구하는 일은 적분을 동반한다. 4장에서처럼 사전확률과 데이터 분포의 결합을 적분하는 일이 수식으로 정리되어 사후 추정치를 구할 수 있다면 행운이다. 
이와 달리 적분이 복잡해 수식을 정리하기 힘든 경우 MCMC 샘플링을 통해 사후모수의 표본을 추출할 수 있다. 
사후 모수의 표본이 있다는 것은 근사적으로 사후모수의 분포를 알며, 각종 통계량(평균, 분산, 신뢰구간 등)을 구할 수 있다는 것을 의미한다.
<br/><br/>
MCMC 시뮬레이션을 설명하기에 앞서 몬테카를로방법과 마코브체인의 개념을 알아보겠다.

(1) Monte Carlo Method <br/>
표본수가 커짐에 따라 랜덤표본에 어떤 함수 $ h $ 를 취한것의 '평균'이 모집단에 $ h $ 를 취한 것의 '기대값'에 근접하는 것을 몬테카를로 방법이라 부른다. <br/>
- $ h $: 임의의 함수
- $ f $: 확률변수 $ X $ 의 확률밀도함수
- $X_1, X_2, ..., X_n $: 확률변수 $ X $ 로부터 i.i.d. 랜덤샘플링으로 뽑힌 표본 
> <center> $ \hat{\mu_{MC}} = \frac{1}{n} \sum^{n}_{i=1} h(X_i) \to \int h(x)f(x) dx = \mu $  as $ n \to \infty $ </center>

[참고. 대수의법칙(LLN, Law of Large Number)] <br/>
표본의 크기가 커질 때 표본평균이 모평균에 근접, $ \bar{X}_n $ 는 샘플 평균, $ \mu $ 는 모평균일 때<br/>
<center> $ \lim_{n \to \infty} P(\bar{X}_n -\mu < \epsilon) =1 $ </center> <br/>

(2) Markov Chain <br/>
마코브 체인은 마코브성질(직전 하나의state만 기억하는 memoryless 성질)을 가진 확률과정을 의미한다. 즉, n+1번째 상태의 값인 $ X_{n+1} $ 의 값은 직전 상태인 $ X_n $ 에만 의존한다.
상태 공간이 이산일 때 마코브체인을 수식화하면 다음과 같다. <br/>
<center> $ P(X_{n+1}|X_1=x_1, ..., X_n=x_n ) = P(X_{n+1}|X_n=x_n) $ </center>
cf. 베이지안에서 사용하는 마코브체인은 사후분포가 정상분포(stationary distribution)일 때 적용한다. 정상분포란 시점에 상관없이 유한한 transition 횟수 후에 원래 상태로 돌아오고(positive recurrent), 모든 상태가 상호 도달 가능하며(irreducible), 주기적으로 원래 상태가 되지 않는(aperiodic) 전이확률분포를 일컫는다. 이상의 설명은 확률과정론을 찾아보길 바란다. 
 
<br/>
다시 돌아와 MCMC 시뮬레이션을 통한 사후 모수의 추정을 알아보겠다.
사후 모수를 $ \theta' $ 라 하자. MCMC 시뮬레이션을 통해 사후 모수의 표본 $ \theta_1' $ , ..., $ \theta_n' $ 을 뽑는다. 
표본의 기대값 $ \bar{\theta_n'} $  을 $ \mu_{\theta'} $ 의 추정치 $ \hat{\theta'} $ 로 사용한다. 
- MCMC <br/>
위에서 설명한 (1)과 (2)를 종합하여 MCMC 베이지안 사후표본 샘플링을 설명하자면, 
 (2)에 의해 k+1번째 표본인 $ \theta_{k+1}' $ 는 직전에 뽑힌 k번째 표본인 $ \theta_k' $ 에 의존하여 샘플링된다. 
 또한 (1)에 의해 사후표본의 평균 $ \bar{\theta_n'} $ 는 샘플의 크기가 커짐에 따라 참값 $ \mu_{\theta'} $ 에 근사한다. 
 덧붙여, 사후 모수의 표본 분포를 알고 있으므로 사후 모수 추정의 분산과 신뢰구간 등의 통계량을 구할 수 있다.
- Acceptance Rate <br/>
현재 사후 샘플을 $ \theta_k' $ 라 할 때, MCMC 시뮬레이션을 통해 뽑은 사후 샘플 후보를 $ \theta_{k+1}' $ 로 업데이트할지 현재값을 유지할지 채택한다. 
이 때, 채택확률이 1 이하 일 때의 MCMC 시뮬레이션 **메트로폴리스-헤이스팅스 알고리즘**이라 부르고, 
 채택확률이 1일 때 MCMC 시뮬레이션을 **깁스샘플링**이라 부른다. 즉 깁스샘플링은 메트로폴리스-헤이스팅스 알고리즘의 특수한 케이스다.

## 6.1. Metropolis-Hastings

현재 상태를 t, 다음 상태를 t+1이라 하자. 그리고 $ \theta \sim f(\theta) $ 이고 제안분포(proposal distribution가 $ g $ 라 하자. 
(제안분포는 이전 현재값 $ \theta^{(t)} $ 과 다음값의 후보값 $ \theta^* $ 간의 조건부 확률을 의미한다.) 

1. sample $ \theta^* $ from $ f( \theta^* | \theta^{(t)} ) $ <br/>
 x(사전확률)가 주어졌을 때, 다음 iteration step의 후보값 $ \theta^* $ 를 분포 $ g $ 로부터 뽑는다. 
2. Metropolis-Hastings ratio 계산 <br/>
$ R( \theta^{(t)}, \theta^* ) =   \frac{ f( \theta^* ) g( \theta^{(t)} | \theta^* ) }{ f( \theta^{(t)} ) g( \theta^* | \theta^{(t)}) } $  <br/>
메트로폴리스 헤이스팅스 알고리즘은 $ g $ 가 symmetric한 경우의 마코브체인이다. 즉, $ g( \theta^{(t)} | \theta^* ) = g( \theta^* | \theta^{(t)} ) $ . 따라서
$ R( \theta^{(t)}, \theta^* ) = \frac{ f( \theta^* ) }{ f( \theta^{(t)} ) } $ <br/>
3. $ R( \theta^{(t)} , \theta^* ) $ 에 따라 업데이트를 결정한다. <br/>
$ \theta^{(t+1)} = \theta^* $ with probability $ min( 1, R( \theta^{(t)}, \theta^* )) $ otherwise $ \theta^{(t)} $

데이터가 $ y $ 라 할 때, 베이지안 추론에서는 모수 $ \theta $ 의 확률분포 $ f $ 를 사후확률분포  $ \pi(\theta | y) $ 로 설정하고, 
sample $ \theta^* $ 를 추출하는 제안분포 $ g $ 를 사후분포 $ \pi(\theta^*) $ 로 설정한다.
