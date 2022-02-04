---
title: Time Series Analysis
category: Theory
order: 6
use_math: true
comments: true
---

# Reference
통계에서의 시계열 분석 이론을 다룬다. 본 포스팅의 내용은 신동완 교수님 수업의 요약본이다.

[사전지식] 기초통계학, 확률론

# Contents
1. [기본 개념](#1.-기본-개념) <br/>
2. [정상 시계열 모형](#2.-정상-시계열-모형) <br/>
3. [비정상 시계열 모형](#3-비정상-시계열-모형) <br/>
4. [예측](#4-예측) <br/>
5. [모형식별과 선택](#5-모형식별과-선택) <br/>
6. [이분산성과 자기상관성](#6-이분산성과-자기상관성) <br/>
7. [조건부 이분산성](#7-조건부-이분산성) <br/>
8. [Value at Risk](#8.-Value-at-Risk)
9. [연속시계열](#9.-연속시계열)

---

# 1. 기본 개념

$ Z_t $ : 시계열, $ t = 1, 2, 3,...$ , t는 시간 단위(일, 분기 등)

[Stationarity(정상성)] <br/>

"확률적"으로 과거와 미래가 동일, 주기적 변동이 없음

- 강정상성(Strong Stationarity)
    - 결합확률분포가 일정: $ (Z_{t_1}, ..., Z_{t_n}) \stackrel{d}{=} (Z_{t_1 + k}, ..., Z_{t_n + k}) $ for all $k, t_1, ..., t_n$
    - 즉,  $ F(Z_{t_1}, ..., Z_{t_n}) = F(Z_{t_1 + k}, ..., Z_{t_n + k}) $

- 약정상성(Weak Stationarity)
    - 결합 분포의 요약이 일정(평균, 분산, 상관계수가 일정)
    - 시계열의 평균이 시점에 의존하지 않음
    - 두 시점의 상관계수가 시점이 아니라 시간 차이에만 의존

[ACF, PACF] <br/>

확률변수가 평균과 분산을 중요 모수로 갖는다면, 정상시계열은 ACF와 PACF를 중요 모수로 갖는다.

- 자기상관함수(Autocovariance and Autocorrealtion Function, ACF)
    - 자기공분산함수: $ \gamma_k = Cov(Z_t, Z_{t+k}) $ <br/>
        참고. $ \gamma_0 = Var(Z_k) = Var(Z_{k+1}) $
    - 자기상관함수: $ \rho_k = \frac{Cov(Z_t, Z_{k+1})}{\sqrt{Var(Z_t)Var(Z_{t+k})}} = \gamma_k / \gamma_0 $
    - ACF는 중요 성질들을 가진다. 그 중 가장 기억해야 하는 것은 $ \gamma_k = \gamma_{-k} $ , $ \rho_k = \rho_{-k}$ 이다. <br/>
    $\because  \gamma_k = Cov(Z_t, Z_{t+k})  = E(Z_t - \mu)(Z_{t+k} - \mu) = E(Z_{t+k} - \mu)(Z_t - \mu) = \gamma_{-k}  $

- 편자기상관함수(Partial Autocorrelation Function, PACF)
    - $ \phi_{kk} = Corr(Z_t, Z_{t+k} | Z_{t+1}, ..., Z_{t+k-1}) = Corr(e_t, e_{t+k}) $
    - $  Z_{t+1}, ..., Z_{t+k-1} $ 를 통제 후 $ Z_t, Z_{t+k} $ 의 상관계수
    - 구하는 법 <br/>
    $ Z_t \sim   Z_{t+1}, ..., Z_{t+k-1} $ 회귀분석 -> 예측오차 $ e_t = Z_t - \hat{Z_t} $ 구함 <br/>
    $ Z_{t+k} \sim   Z_{t+1}, ..., Z_{t+k-1} $ 회귀분석 -> 예측오차 $ e_{t+k} = Z_{t+k} - \hat{Z}_{t+k} $ 구함 <br/>
    $ Corr(e_t, e_{t+k}) $ 구함
    - ACF를 이용해 PACF를 축차적으로 구하는 알고리즘: [Durbin Levinson Recursion](https://en.wikipedia.org/wiki/Levinson_recursion)

[White Noise Process]
- $ Z_t $ 의 평균이 0, 분산이 $ \sigma^2 $, uncorrelated($\rho_k=0$)
- $ Z_t $ 의 분포가 정규분포이면 가우시안 백색잡음과정이라 함

[정상시계열의 모수 추정]
- 표본 $Z_1, ..., Z_n $ 으로부터 모수를 추정한다. 
- 확률변수에서 모수를 추정하고, 추정치의 평균과 분산, 나아가 분포를 구하는 과정이 시계열변수에서도 동일하게 적용된다. 따라서 다음 내용을 이해하려면 표본으로 구한 모수 추정치의 분포에 대한 개념을 잘 알아야 한다. <br/>
*표본이란 모집단에서 랜덤하게 혹은 어떤 규칙에 의해 추출한 관측치의 집합이다. 따라서 모집단의 모수들을 추정한 추정치는 모수를 맞추는 적확한 값이 아니라 표본이 어떻게 뽑혔느냐에 따라 달라지는 값이다. 이 변동성 때문에 추정치는 분포를 가진다.  

- 표본평균: 
    - $ \hat{\mu} = \bar{Z} = \frac{1}{n} \Sigma^n_{t=1} Z_t $
    - 성질: 일반적인 표본평균의 추정치를 구하는 것과 동일하다. <br/>
    표본평균의 불편성 $ E(\bar{Z}) = Z_t $ <br/>
    표본평균의 분산: $ Var(\bar{Z}) = \gamma_0 / n + \alpha $ , 독립샘플이면 $ \gamma_0 / n $ <br/> 
    표본평균의 일치성: $ \rho_k=0 $ 이면 일치성 가짐 <br/> 
    표본평균의 최적성: 샘플 크기가 크면 최적에 가까움 <br/>

    *일치성: 표본 크기가 커지면 추정치가 모수에 가까워지는 성질, 여기서는 표본평균이 모평균에 근사한다는 의미 <br/>
    *최적성: 시계열을 $ \dot{Z_n} = \mu 1_n+ $ error term, $\dot{Z_n} = (Z_1, ,..., Z_n)^T$ 으로 표현할 수 있는데 이를 회귀분석으로 치환해 생각 <br/> 
    $ y $ (종속변수)는  $ \dot{Z_n} $ , $ \beta $ (회귀계수)는 $ \mu $, $ X $ (독립변수)는 $ 1_n $ <br/>
    -> $\mu$를 회귀분석에서 추정할 계수(통상 $\beta$)라고 생각 <br/> 
    -> 최소제곱추정치(BLUE라는 성질 떄문에 회귀분석에서는 최적의 추정치)는 $ \hat{\mu} = (1_n'1_n)^{-1}(1_n\dot{Z}_n) = \bar{Z} $

표본 ACF, PACF 성질 [참고](http://feldman.faculty.pstat.ucsb.edu/174-03/lectures/l12.pdf)
- Sample Autocovariance:
    - 모수(모집단 이용): $ \gamma_k = Cov(Z_t, Z_{t+k}) = E(Z_t -\mu )(Z_{t+k} -\mu ) $
    - 추정치(sample 이용): $ \hat{\gamma}_k = \frac{1}{n} \Sigma^{n-k}_{t=1}(Z_t-\bar{Z})(Z_{t+k}-\bar{Z}) $ 
    - 샘플 크기가 크면 위 식에서 분모를 $n-k$ 가 $n$ 으로 해도 된다. (bias 무시 가능)

- Sample Autocorrelation
    - $ \hat{\rho}_k = \frac{\gamma_k }{\gamma_0}/  = \frac{ \Sigma^{n-k}_{t=1}(Z_t-\bar{Z})(Z_{t+k}-\bar{Z}) }{ \Sigma^{n}_{t=1}(Z_t-\bar{Z})^2 }   $
    - $ \rho_k = 0 $ 이면 $ Var(\hat{\rho}_k ) \approx \frac{1}{n}(1 + 2\rho_1^2 + 2\rho_2^2 + ... + 2\rho_m^2) $ for $k>m$ <br/> -> k차까지 계산 안하고 적당한 시점 m까지만 계산
    - white noise의 경우 $ \hat{\rho} = \frac{1}{n} $


# 2. 정상 시계열 모형