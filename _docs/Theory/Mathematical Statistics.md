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

- 통계학에서 자주 사용되는 기초적인 성질과 theorem을 다룬다.
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
분포의 가장 중요한 성질 중 하나는 적률생성함수(MGF, Moment Genrating Function)다. 그 이유는 분포와 MGF는 unique하게 1:1 대응이기 때문이다. 즉, 특정 확률변수의 MGF를 알면 역으로 분포를 알 수 있다. 또한 분포의 여러가지 통계량을 구하거나 증명을 할 때 mgf가 유용하게 이용된다.
<br/>  
[적률생성함수 정의]
> <center>$ M(t)=E( e^{tX} )  = \sum_x e^{tX} p(x) $ <center/>

위에서 언급했듯이 분포와 MGF는 1:1 대응이다. 따라서 두 확률변수 $ X $, $ Y $가 동일한 확률분포를 가지면 두 확률변수의 MGF 역시 동일하다.
> 성질1. $ F_X(z) = F_Y(z)$ for all $ z \in \mathbb{R} $ if and only if $ M_X(t) = M_Y(t) $ for all $ t \in (-h,h) $ for some $h>0$  

<br/>
MGF의 이름처럼 $ M_X(t) $ 를 m차 미분하면 확률변수의 m차 기대값을 생성할 수 있다. 
m차 기대값을 구할 때 exponential 함수는 미분꼴이 자기자신과 같고, $e^0=1$이라는 성질을 이용한다. 
  
> 성질2. $ E(X^m) = \sum_x x^m p(x) $ or $ \int_{-\infty}^{\infty} x^m f(x) $ 
  
- 1차 미분 <br/>  
$ M'(t) = \frac{M(t)}{dt} = \sum_x xe^{tx}p(x) $ or $ \int_{-\infty}^{\infty} x e^{tx}f(x) dx $ <br/>
여기에 $ t=0 $ 을 대입하면 $ M'(0) = E(X) =\mu $
  
- 2차 미분 <br/>
$ M^{''}(t) = \sum_x x^2 e^{tx}p(x) $or $ \int_{-\infty}^{\infty} x^2 e^{tx}f(x) dx $ <br/>
여기에 $ t=0 $ 을 대입하면 $ M^{''}(0) = E(X^2) $ 
  
- 분산 <br/>
위에서 구한 1차, 2차 적률을 이용해 분산을 구할 수 있다. $ \sigma^2 = E(X^2)-E(X) = M''(0) - M'(0) $
<br/>  
  
  
**[MGF의 유용한 활용]** <br/> 
$ X_1, X_2, ..., X_n $ 이 identically independent distributed(iid) $ F $ 를 따른다고 가정하자. ( $ F $ 는 임의의 확률분포) 
1. 합의 분포 <br/>
  $ S_n = X_1 + X_2 + ... + X_n $ 의 mgf는 $ E(e^{t S_n}) = E(e^{t X_1}...e^{t X_1}) $ 인데 
  독립성질에 의해 기대값이 분해되어  $ E(e^{t X_1}) ...E(e^{t X_n}) $ 가 되고
  확률변수들이 동일분포를 따르므로 $  E(e^{t X_i})^n = M_X(t)^n $ 로 정리된다.
> 참고. MGF of $ Z \sim  N(\mu, \sigma^2) $: $ M_Z(t) = exp(\mu t + \frac{\sigma^2 t^2}{2}) $ 
  
2. MGF를 이용한 중심극한정리(CLT) 증명 <br/> 
  중심극한정리는 표본의 수가 커질수록 '평균의 분포'가 정규분포를 따른다는 정리다. 표본수가 커짐에 따라 '합의 분포'가 정규분포에 근사한다는 것을 보임으로서 합을 표본수로 나눈 '평균의 분포' 역시 정규분포로 근사한다는 사실을 MGF를 통해 증명하겠다. <br/>
  
  [step1] <br/>
  $ S_n = X_1 + X_2 + ... + X_n $ 일 때 <br/>
  $ Z_n  =  \frac{S_n- n \mu}{\sqrt{ n \sigma^2 }} $ (표준화된 합의 분포)
  $ = \sum_{i=1}^n \frac{ X_i - \mu }{ \sigma \sqrt{n} } $ 
  $ = \frac{ S_n' }{ \sigma \sqrt{n} }  $ , <br/> 
  where $ S_n' = \sum_{i=1}^n ( X_i - \mu ) = \sum X_i^* $ 
  (demeaned $ X_i $ 를 $ X_i^* $ 라 하자) <br/>
  이 때 $ Z_n $ 의 MGF는 다음과 같다.  <br/>
  $ M_{Z_n}(t) = E(exp(t Z_n)) = E( exp( t \frac{S_n'}{ \sigma \sqrt{n} } ) ) $  <br/>
  $ = E( exp( \sum_{i=1}^n X_i^* \frac{ t }{ \sigma \sqrt{n} } ) )  =  ( M_{X^*}( \frac{t}{ \sigma \sqrt{n} }  ) )^n $ <br/> 
  
  1번에 의해 합의 분포 $ S_n' $ 의 MGF가 개별 분포 $ X_i^* $ 의 MGF의 n차로 분해된다. <br/>
  
  [step2] <br/>
  위에서 도출한 
  $ M_{Z_n}(t) =  ( M_{X^*}( \frac{t}{ \sigma \sqrt{n} }  ) )^n $ 
  를 $ t=0 $ 에 대해 2차 테일러 전개를 한다.  <br/> 
  
  $ M_{Z_n}(t) \approx $ 
  $ ( M_{ X^* } (0) + \frac{ t }{\sigma \sqrt{n} } M_{ X^* }'(0) + \frac{ t^2 }{ 2 n \sigma^2 }  M_{ X^* } '' (0) )^n $ <br/>
  $ = ( 1+ \frac{t^2}{2 n \sigma^2 } \sigma^2  )^n $, 
  <br/>
  since
  - $ M_{X^*}(0) = E(e^0)=1 $ 
  - $ M_{X^*}'(0) = \mu = 0 $ 
  - $ M_{X^*}''(0) = \sigma^2 $ 
  
  $ =  ( 1+ \frac{t^2}{2 n }  )^n  \rightarrow e^{t^2/2}  $ as $ n  \rightarrow \infty $ 
  
  마지막에 도출된 $ e^{t^2/2} $ 이 $ N(0,1) $ 의 MGF다.

  
### 특성함수
확률분포에 따라 MGF가 존재하지 않는 경우가 있다. 어떤 분포에 대해서도 특성함수(CF, charateristic function)은 존재하기 때문에 MGF가 존재하지 않을 경우 CF를 사용한다. 
<br/>  
[특성함수의 정의]
> <center>$ \phi(t) = E( e^{itX} )  = \sum_x e^{itX} p(x) $ <center/>

적률생성함수에서 설명한 성질들이 특성함수에도 동일하게 적용된다
1. $ E( e^{it S_n} ) = \phi(t)^n $
2. CLT 증명 가능
3. m차 모먼트가 유한하다면, $ \phi^{(m)}(t) = E( \partial^m e^{it X_1} / \partial t^m ) = E( (i X_i )^m e^{it X_1} )  $ <br/>
  $ \phi^{(m)}(0) = i^m E(X_1^m) $ <br/>

## 1.10. Important Inequalities
  
1. Jensen's Inequality
 
<img class="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c7/ConvexFunction.svg/1280px-ConvexFunction.svg.png"  width=550px/>
  
> <center> If $ \phi $  (strictly) convex function, then $ \phi( E(X) ) \geq E(\phi(X)) $ <center/>     

어떤 함수가 convex(아래로 볼록)일 때 함수값의 평균이 평균의 함수값보다 크거나 같다. 이미지를 보면 젠센 부등식을 쉽게 이해할 수 있는데 함수값의 평균은 핑크색 선 위에 있고 평균의 함수값은 검정색 선 위에 있는데 두 점 사이의 구간에서 항상 핑크색 선이 검정색 선보다 위에 있다.
위 부등식의 증명은 [강의노트](https://www.cs.purdue.edu/homes/hmaji/teaching/Spring%202018/lectures/note01.pdf) p5-p6에서 설명한다.
$ \phi(x) $ 식에 대하여 $ X=\mu $ 에서의 2차 테일러 전개를 이용해 증명한다.
  
2. Markov Inequality
3. Chebyschev Inequality
4. Cauchy-Schwarz Inequality
5. Shannon Inequality
