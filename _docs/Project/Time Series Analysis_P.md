---
title: Time Series Analysis(적용)
category: Project
order: 1
use_math: true
comments: true
---


# Contents
1. [통계학 관점 요약](#1-통계학-관점-요약) <br/>
2. [딥러닝 관점 요약](#2-딥러닝-관점-요약) <br/>
3. [공통점과 차이점](#3-공통점과-차이점) <br/>
4. [고려사항](#4-고려사항) <br/>
5. [프로젝트 목록](#5-프로젝트-목록) <br/>

- 정형 데이터에 한정해 시계열 모형을 서술했다.
- 각 이론의 자세한 내용은 Theory를 확인한다.

------

# 1.통계학 관점 요약

## 들어가기 전에
- B: 시점을 뒤로 미는 후행연산자(backshift),  $ y_{t-1} = By_t $
- $ a_t $ : white noise, 주로 $ N(0,\sigma^2) $ , <br/>
단 시점에 따라 분산이 안바뀌는 경우(등분산) $ \sigma^2 $ (상수인 분산), 시점에 따라 바뀌는 경우(이분산) $ \sigma_t^2 $  
- 참고. 반드시 오차항 $a_t$ 가 가우시안 노이즈는 아님
- 각 모형 별 발생 가능한 그래프의 패턴을 숙지해야 한다.

## BOX-JENKINS 모형 총정리
$a_t$ 는 white noise 가정

- AR(p): ACF 서서히 감소, 시점 p에서 PACF 절단
> <center> $ y_t = \phi_1 y_{t-1} + ... + \phi_{t-p} y_{t-p} + a_t $ </center>
> <center> $ \Leftrightarrow  (1-\phi_1 B + ... + \phi_{t-p} B^p)y_t = a_t $ </center>

- MA(q): 시점 q에서 ACF 절단, PACF 서서히 감소
> <center> $ y_t = a_t - \theta_1 a_{t-1}...-\theta_{t-q} a_{t-q} $ </center>
> <center> $ \Leftrightarrow y_t = (1-\theta_1 B...-\theta_{t-q} B^q )a_t $ </center>

- ARMA(p,q): 시점 p에서 PACF 절단, 시점 q에서 ACF 절단
> <center> $ y_t = \phi_1 y_{t-1} + ... + \phi_{t-p} y_{t-p} + a_t - \theta_1 a_{t-1}...-\theta_{t-q} a_{t-q} $ </center>
> <center> $ \Leftrightarrow (1-\phi_1 B + ... + \phi_{t-p} B^p)y_t = (1-\theta_1 B...-\theta_{t-q} B^q )a_t $ </center>

- ARIMA(p,d,q)
  - d회 차분 
  - 비정상 계열, ACF 느리게 감소, 단위근이 있음 ( $1-B$ )
  - 그래프 또는 단위근 검정(Dickey Fuller test) 통해 비정상성 확인
> <center> $ (1-\phi_1 B + ... + \phi_{t-p} B^p)(1-B)^d y_t = (1-\theta_1 B...-\theta_{t-q} B^q )a_t $ </center>

- 분산 안정화
  - $ y_t $ 의 분산이 클 경우 box-cox 변환을 통해 분산 안정화 <br/> 
  ex. $ ln(y_t), \sqrt(y_t), (y_t^\lambda-1)/\lambda $
- 계절성 
  - 계절 주기 s마다 반복되는 특징이 보임
  - 계절 주기 s만큼 ARIMA term에 곱함 $ (1-B^s) $ 
  - $ ARIMA(p,d,q) \times (P,D,Q)_s $
  - 예. 계절주기가 4 term 마다 반복될 경우, $ IMA(1,1)\times (1,1)_4 $ : $ (1-B)(1-B^4)y_t = (1-\theta B)(1- B^4)a_t $

- ARIMAX
  - 외생변수(공변량, covariate)를 포함한 ARIMA, 예를 들어 실업률 시계열 모형에 GDP 변수를 추가
  - ARIMA 모형에 추가 변수( $ x_1, ..., x_k $ )를 포함
  - ex. ARMAX: $ y_t = \beta_1 x_t + \phi_1 y_{t-1} + a_t - \theta_1 a_{t-1} $ 

- ARIMA with Drift
  - ARIMA에 상수항이 포함된 모형이다. 지속적으로 감소, 증가하는 경우 선형적 트렌드를 갖는데 이를 상수항으로 표현한다.

## 조건부 이분산성

- 오차항 $ a_t $ 의 변동성이 이분산인 경우
- 위에서 ARIMA 모형으로 모델링을 했음에도 오차항이 white noise가 아닐 수 있다. 즉 모델이 데이터를 설명하고 남은 나머지가 랜덤이 아니므로 추가적으로 오차항에 대한 모델링이 필요하다.
- 변동성은 시점 cluster 별로 다른 경우가 있으며(volatility clustering), 현재의 변동성은 미래 시점의 변동성으로 전이된다. 예를 들면 경기 수축기와 확장기의 시계열의 변동폭이 다르며 인접한 시점간 $y_t$의 자기상관이 매우 높다.
- (1) 시간 구간 별(ex. 경기 수축기 vs 경기 확장기) 분산이 이분산(등분산 검정비)이고 <br/>
(2) 인접 값 별 자기상관성이 높으면 <br/> ARCH 또는 GARCH 모형을 적합한다. 

- 오차항 $ a_t \sim N(0, \sigma_t^2) $
  -  즉 오차항의 분산이 시점에 따라 바뀌며 현재의 분산 즉 변동성은 과거 오차항 $ a_k $ 또는 과거 변동성 $ \sigma^2_k $에 의존
- ARCH(p)
> <center> $ \sigma^2_t =  \beta_0+\beta_1 \sigma^2_{t-1}+...+ \beta_{t-p} \sigma^2_{t-p}  $ </center>

- GARCH(p,q)
> <center> $  \sigma^2_t =  \alpha_0 + \alpha_1 a_{t-1} + ... +  \alpha_p a_{t-q} +  \beta_1 \sigma^2_{t-1}+...+ \beta_{t-p} \sigma^2_{t-p}  $ </center>

- ex. GARCH(1,1): $ \sigma_t^2 = \alpha_0 + \alpha_1 a_{t-1}^2 + \beta_1 \sigma_{t-1}^2   $

- 추가. 변동성 모형의 계수의 크기를 제한하거나(EGARCH) 양수의 shock이나 음수의 shock을 다르게 모델링(QGARCH)하는 등 다양한 옵션에 따라 여러 종류의 GARCH 모형이 있다.

## 종합

- ex. ARIMA(1,1,1) + GARCH(1,1)이면
> <center> $ (1-\phi B )(1-B)y_t = (1-\theta_1 B)a_t $ </center>
> <center> where $ a_t \sim N(0,\sigma_t^2) $ </center>
> <center> $ \sigma_t^2 = \alpha_0 + \alpha_1 a_{t-1}^2 + \beta_1 \sigma_{t-1}^2 $ </center>

- 참고. ARIMA의 차수 p,d,q  등 각 차수는 너무 크지 않은 것이 바람직
- 참고. best 모델 선택 기준은 AIC

----

# 2. 딥러닝 관점 요약

- 딥러닝 모형의 뿌리는 합성함수다.
- 시계열에서의 딥러닝은?
  - $ X = (X_1, X_2, X_3,...) $ 가 feature라 할 때 k개의 레이어를 가진 다중퍼셉트론의 경우 $f_k( f_{k-1}(...f_1( X))$ 로 표현된다. 즉 변수 간 관계에 방향성은 없다. (교호작용 항이 포함될 지언정 첫번째 피쳐가 두번째 피쳐에, 두번째 피쳐가 세번째 피쳐에 영향을 주는 관계는 아님) 반면, 시계열 딥러닝 모형은 시점을 앞 시점의 피쳐가 뒤 시점의 피쳐에 영향을 주는 관계다.
  - 시계열에서는 5개의 과거시점이 미래의 1시점을 결정한다고 할 때, $ (x_{t-5}, x_{t-4}, x_{t-3}, x_{t-2}, x_{t-1}) $로 피쳐를 표현할 수 있다. <br/>
  - RNN <br/>
activation 역시 일종의 함수므로 $ f $라고 표현하면 앞 시점의 피쳐의 선형결합에 어떤 함수를 취해 나온 값을 다음시점의 선형결합에 사용한다. 즉 RNN은 다중퍼셉트론과 달리 시간이라는 피쳐 간 관계를 표현하기 위해 앞 시점의 합성함수가 뒷시점에 영향을 주도록 모델이 만들어진다. <br/>
$  h_{t-4} = f_5 ( W_h h_{t-5}+ W_x x_{t-5} ) $ <br/>
$  h_{t-3} = f_4 ( W_h h_{t-4}+ W_x x_{t-4} ) $ <br/>
$ = f_4 ( W_h f_5 ( W_h h_{t-5}+ W_x x_{t-5} )+ W_x x_{t-4} ) $  <br/>
... <br/>
$  h_{t-1} = f_1 ( W_h h_{t-1}+ W_x x_{t-1} ) $ <br/>
다만 위에서 설명한 RNN을 한 층 적용했을 때의 이야기이다. RNN을 두 개의 층을 적용했다고 가정햇을 때, $ x_{t-5} $는 첫번째 층의 다음시점인 $ h_{t-4}^{(1)} $ 뿐만 아니라 자기 자신 시점의 다음 층인 $ h_{t-5}^{(2)} $에 영향을 미친다. 따라서 여러 층의 RNN을 만들 경우 각 시점에서의 $ h_{k} $ 를 다음 층에서 사용해야 하기 때문에 return_sequences=True(각 시점에서 활성 함수를 거친 결과값을 반환)로 설정해야 한다.
- 결론적으로 딥러닝에서의 시계열 모형은 앞시점의 피쳐가 뒷 시점의 피쳐에 영향을 주도록 합성함수를 설계한다.

---

# 3. 공통점과 차이점
다음 내용은 글쓴이 스스로의 생각과 [논문](https://repository.upenn.edu/cgi/viewcontent.cgi?article=1166&context=wharton_research_scholars)을 참조해 서술했다.

## 3.1. 공통점
- 통계학 모델, 딥러닝 모델 모두 최근 몇 시점의 과거값이 현재 시점의 값을 결정하는 구조다.

## 3.2. 차이점
- 통계학 방법은 term(예, AR term, MA term...)을 추가할 때마다 ACF, PACF를 확인하거나 Dickey Fuller test와 같이 각종 검정 과정을 거쳐야 한다.
- 통계학 방법은 적확한 예측 보다는 과거값이 미래 값에 미치는 구조를 확률분포를 포함하여 설명한다. 따라서 예측치(point estimation)만큼이나 예측 구간을 확인하는 것이 중요하다.(미래 값의 후보를 결정하는 '변동성'이 중요시 됨) <br/> 
반면 딥러닝 관점은 손실함수가 예측 오차를 줄이는 방향으로 움직이는 만큼 통계학 방법보다 적확한 예측을 추구한다. 그러나 시점에 따라 변화하는 값의 변동성을 모델에 반영하지는 않는다.
- 통계학 방법은 과거 오차항이든 과거 피쳐 값이든 선형결합으로 과거값이 반영된다. 반면 딥러닝 방법은 과거 값을 비선형으로 반영해 예측할 수 있다.

<br/>
예측력 면에서 통계학 방법은 딥러닝 방법에 비해 성능이 좋지 않을 수 있다. 그러나 통계학 방법이 열등한 것은 아니다. 통계학적 방법은 시점에 따른 변화의 확률 분포까지 고려하므로 시뮬레이션이 가능하다. 위 단락에 서술하지는 않았지만, 통계학 모형 중 연속시계열 모형의 Value at Risk는 시뮬레이션으로 발생 가능한 결과 중 분위수 하위 n%일 때 발생 가능한 값을 제시해준다. 따라서 목적에 맞게 방법론을 선택하면 된다. 예컨대, 수요 예측 분야에서는 비선형 모델의 예측이 필요해 딥러닝을 사용할 수 있다. 반면 금융 분야에서 주식 포트폴리오를 만들 때 가능한 결과 중 최대 손실을 줄이고자 통계학적 방법을 사용할 수 있다. 

---

# 4. 고려사항

- 학습 데이터의 사이즈(통계), 룩백 윈도우 사이즈(딥러닝)
- 예측할 시점(얼마나 먼 미래를 예측해야 하는지)
- 외생변수 포함, 단 미래 예측시 외생변수의 미래값이 필요 
- 예측의 목적에 맞는 분석 방법 선택
  - 장기 트렌드 예측 -> 스무딩 후 예측, shock 예측 -> 극값분포의 loss 사용 등

---

# 5. 프로젝트 목록
- 파이썬을 이용해 시계열 분석을 시도했다.

1. [S&P 500 예측](https://github.com/yrk3434/Open_Archive/blob/main/Time%20Series%20Analysis/S%26P500_prediction.ipynb)
  - ARIMA + GARCH와 RNN을 이용해 예측 후 비교했다.
