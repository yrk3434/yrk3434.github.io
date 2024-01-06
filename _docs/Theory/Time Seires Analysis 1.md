---
title: Time Series Analysis
category: Theory
order: 6
use_math: true
comments: true
---

# Reference
통계에서의 시계열 분석 이론을 다룬다. 본 포스팅의 내용은 신동완 교수님 수업의 요약본이다.

[사전지식] 기초통계학, 확률론, 회귀분석

# Contents
1. [기본 개념](#1-기본-개념) <br/>
2. [정상 시계열 모형](#2-정상-시계열-모형) <br/>
3. [비정상 시계열 모형](#3-비정상-시계열-모형) <br/>
4. [예측](#4-예측) <br/>
5. [모형식별과 선택](#5-모형식별과-선택) <br/>
6. [이분산성과 자기상관성](#6-이분산성과-자기상관성) <br/>
7. [조건부 이분산성](#7-조건부-이분산성) <br/>
8. [Value at Risk](#8.-Value-at-Risk)
9. [연속시계열](#9.-연속시계열)

------

# 1. 기본 개념

$ Z_t $ : 시계열, $ t = 1, 2, 3,...$ , t는 시간 단위(일, 분기 등)

## 1.1 Stationarity(정상성)

"확률적"으로 과거와 미래가 동일, 주기적 변동이 없음

- 강정상성(Strong Stationarity)
    - 결합확률분포가 일정: $ (Z_{t_1}, ..., Z_{t_n}) \stackrel{d}{=} (Z_{t_1 + k}, ..., Z_{t_n + k}) $ for all $k, t_1, ..., t_n$
    - 즉,  $ F(Z_{t_1}, ..., Z_{t_n}) = F(Z_{t_1 + k}, ..., Z_{t_n + k}) $

- 약정상성(Weak Stationarity)
    - 결합 분포의 요약이 일정(평균, 분산, 상관계수가 일정)
    - 시계열의 평균이 시점에 의존하지 않음
    - 두 시점의 상관계수가 시점이 아니라 시간 차이에만 의존

## 1.2. ACF, PACF

확률변수가 평균과 분산을 중요 모수로 갖는다면, 정상시계열은 ACF와 PACF를 중요 모수로 갖는다.

- 자기상관함수(Autocovariance and Autocorrealtion Function, ACF)
    - 자기공분산함수: $ \gamma_k = Cov(Z_t, Z_{t+k}) $ <br/>
        참고. $ \gamma_0 = Var(Z_k) = Var(Z_{k+1}) $
    - 자기상관함수: $ \rho_k = \frac{Cov(Z_t, Z_{k+1})}{\sqrt{Var(Z_t)Var(Z_{t+k})}} = \gamma_k / \gamma_0 $
    - ACF는 중요 성질들을 가진다. 그 중 가장 기억해야 하는 것은 $ \gamma_k = \gamma_{-k} $ , $ \rho_k = \rho_{-k}$ 이다. <br/>
    $\because  \gamma_k = Cov(Z_t, Z_{t+k})  = E(Z_t - \mu)(Z_{t+k} - \mu) = E(Z_{t+k} - \mu)(Z_t - \mu) = \gamma_{-k}  $

- 편자기상관함수(Partial Autocorrelation Function, PACF)
    - $ \phi_{ kk } = Corr( Z_t, Z_{t+k} | Z_{t+1}, ..., Z_{t+k-1} ) = Corr( e_t , e_{ t+k } ) $
    - $  Z_{t+1}, ..., Z_{t+k-1} $ 를 통제 후 $ Z_t, Z_{t+k} $ 의 상관계수
    - 구하는 법 <br/>
    $ Z_t \sim   Z_{t+1}, ..., Z_{t+k-1} $ 회귀분석 -> 예측오차 $ e_t = Z_t - \hat{Z_t} $ 구함 <br/>
    $ Z_{t+k} \sim   Z_{t+1}, ..., Z_{t+k-1} $ 회귀분석 -> 예측오차 $ e_{t+k} = Z_{t+k} - \hat{Z}_{t+k} $ 구함 <br/>
    $ Corr(e_t, e_{t+k}) $ 구함
    - ACF를 이용해 PACF를 축차적으로 구하는 알고리즘: [Durbin Levinson Recursion](https://en.wikipedia.org/wiki/Levinson_recursion)

## 1.3. White Noise Process

- $ Z_t $ 의 평균이 0, 분산이 $ \sigma^2 $, uncorrelated($\rho_k=0$)
- $ Z_t $ 의 분포가 정규분포이면 가우시안 백색잡음과정이라 함

## 1.4 정상시계열의 모수 추정
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
    -> 최소제곱추정치([BLUE](https://statisticsbyjim.com/regression/gauss-markov-theorem-ols-blue/)라는 성질 떄문에 회귀분석에서는 최적의 추정치)는 $ \hat{\mu} = (1_n'1_n)^{-1}(1_n\dot{Z}_n) = \bar{Z} $

표본 ACF, PACF 성질 [참고](http://feldman.faculty.pstat.ucsb.edu/174-03/lectures/l12.pdf)
- Sample Autocovariance:
    - 모수(모집단 이용): $ \gamma_k = Cov(Z_t, Z_{t+k}) = E(Z_t -\mu )(Z_{t+k} -\mu ) $
    - 추정치(sample 이용): $ \hat{ \gamma }_k = \frac{1}{n} \Sigma^{n-k}_{t=1} ( Z_t-\bar{Z} )( Z_{t+k} - \bar{Z} ) $ 
    - 샘플 크기가 크면 위 식에서 분모를 $n-k$ 가 $n$ 으로 해도 된다. (bias 무시 가능)

- Sample Autocorrelation
    - $ \hat{ \rho_k } = \frac{ \gamma_k }{ \gamma_0 }  = \frac{ \Sigma^{n-k}_{t=1} ( Z_t-\bar{Z} )( Z_{t+k} - \bar{Z} ) }{ \Sigma^{n}_{t=1} ( Z_t - \bar{Z} )^2 }   $
    - $ \rho_k = 0 $ 이면 $ Var(\hat{\rho}_k ) \approx \frac{1}{n}(1 + 2\rho_1^2 + 2\rho_2^2 + ... + 2\rho_m^2) $ for $k>m$ <br/> -> k차까지 계산 안하고 적당한 시점 m까지만 계산
    - white noise의 경우 $ \hat{\rho} = \frac{1}{n} $


------


# 2. 정상 시계열 모형

- 다음 AR, MA, ARMA 등의 특성은 정상시계열을 가정한 내용이다. 비정상시계열은 식에 차분이나 트렌드 항을 추가하는 등의 처리를 한 후 정상시계열로 만든 후에 2장에서의 내용을 적용한다.


들어가기 전에: 용어 및 개념
- $ B $ : 후행 연산자(backshift operator), ex. $ Z_{t-1} = B Z_t $
- 특성방정식(Characteristic Equation): $f(B)Z_t = a_t $, $ a_t $는 white noise(error term) <br/> 
여기서 $ B $ 에 관한식 $ f(B) $ 가 특성 방정식이다. <br/>
ex. $ Z_t = \phi_1Z_{t-1} + a_t  \Leftrightarrow ( 1 - \phi_1 B )Z_t = a_t $ <br/> 
$ ( 1 - \phi_1 B ) $ 가 특성방정식 <br/> 

*특성방정식은 선형대수에서 방정식의 해가 존재할 조건을 구할 때 쓰인다. 예를 들어 이차방정식 $ ax^2+bx+c=0 $ 에서 근이 존재할 조건을 $ D = \sqrt{ b^2-4ac } $ 에서 찾는다. $ b^2-4ac >0 $ 이면 실근 2개, 0이면 실근 1개, 0보다 작으면 허근이 존재한다. 시계열분석에서는 AR 계열이 정상이려면 $ f(B) $ 의 식의 판별식인 $ D $ 가 실근이 존재할 조건을 만족해야 한다.

- demeaned process: 평균을 0으로 수정한 계열, 다음 내용에서는 상수항을 제거한 AR 계열을 가정하고 설명한다. <br/>
평균이 0인 것 외의 다른 성질에 대해서 상수항 제거계열은 원계열과 동일한 성질을 갖는다. <br/>
    - 원계열: $ \dot{Z_t} = \phi_0 + \phi_1 \dot{ Z_{t-1} } + a_t $ <br/>
    - 상수항 제거계열: <br/> 
    $ \dot{Z_t} - \mu = \phi_1 ( \dot{ Z_{t-1} } - \mu) + a_t $ <br/>
    $ \Leftrightarrow Z_t = \phi_1 Z_{t-1} + a_t  $ <br/>
-  Yule Walker Equation: 양변에 $Z_t$ 를 곱하고 기대값을 취해 ACF, PACF를 구하는 방법

## 2.1. AutoRegressive(AR) Process
- 자기회귀계열은 변수를 과거값과 선형 관계로 표현하는 것이다.

### 2.1.1. AR(1)
- $ Z_t = \phi_1Z_{t-1} + a_t $ , $ a_t $ 는 white noise
- Yule 과정이라고도 부른다.
- 정상성 조건: $ | \phi_1 | < 1 $ <br/>
    - 위에서 설명한 특성방정식에 따라 $ Z_t = \phi_1Z_{t-1} + a_t  \Leftrightarrow ( 1 - \phi_1 B )Z_t = a_t $ <br/> 
$ 1 - \phi_1 B = 0 $


$ \because $ 정상계열이려면 시계열의 평균과 분산이 존재(발산하면 안 됨) <br/>
    - 평균 <br/>
    식 양변에 기댓값을 취하면, <br/>
    $ E(Z_t) = \phi_1 E(Z_{t-1}) + E(a_t) \Leftrightarrow \mu = \phi_1 \mu $ , 즉 $ \mu $ 는 0 <br/> 
    $ \because \mu = E(Z_t) = E(Z_{t-1})  $ (1.1장 참고, 정상성의 성질 중 평균이 일정하다는 성질이 있음) <br/> 
    & white noise이므로 $ E(a_t) = 0 $
    - 분산 <br/>
    $ var(Z_t) = E(\phi_1Z_{t-1} + a_t)(\phi_1Z_{t-1} + a_t) = \phi_1^2E(Z_{t-1}^2)+2E(Z_{t-1}a_t) + E(a_t^2) $ <br/>
    $ \sigma_z^2 = \phi_1^2 \sigma_z^2 + 0 + \sigma_a^2 $ , $ \because Z_{t-1}, a_t $ 는 서로 다른 시점의 항으로 uncorrelated -> $ E(Z_{t-1}a_t)= 0 $ <br/>
    $ \sigma_z^2 =\sigma_a^2/(1-\phi_1^2) $ 여기서 $ Z_t $ 의 분산인 $ \sigma_z^2 $ 이 존재하려면 <br/>
    $ 1-\phi_1^2 \neq 0   \Leftrightarrow  | \phi_1 | < 1  $

- ACF: $ \gamma_k = \frac{\sigma^2_a}{1-\phi_1^2} \phi_1^k $ <br/>
$ \because Z_t = \phi_1Z_{t-1} + a_t $ <br/>
$ \Leftrightarrow  E(Z_{t-k} Z_t) = E(Z_{t-k} \phi_1Z_{t-1} ) + E(Z_{t-k} a_t )  $ <br/>
$ \Leftrightarrow  \gamma_k = \phi_1 \gamma_{k-1} + 0  $ <br/>
$ \Leftrightarrow  \gamma_k / \gamma_0 = \phi_1 \gamma_{k-1}  / \gamma_0   $ <br/>
$ \Leftrightarrow  \rho_k = \phi_1 \rho_{k-1}  = ... = \phi_1^k \rho_0 = \phi_1^k \because \rho_0=1 $ <- Yule Walker 방정식 <br/>
- PACF: <br/>
$ \phi_{kk} = \rho_1 = \phi_1 $ for $ k=1 $ <br/> 
$ \phi_{kk} =0 $ for $ k \geq 2 $
