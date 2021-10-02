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
- 이 포스팅에서는 확률론, 회귀분석 등 주요 과목과 중복되는 내용은 제외하고, 여러가지 수리적 성질에 관한 내용만 다룬다.
- reference 책 이상의 범위의 내용은 소병수 교수님의 수리통계학/이론통계학 수업을 참조했다.
- 이 과목은 원래 어렵다. 내용이 와닿지 않는 게 당연한 과목이다. 나름대로의 첨언을 통해 정리들의 의미와 의의를 기술했지만 이 마저도 설명이 부족하게 느껴질 게 당연하다. 그래도 수많은 과목의 근간이 되는 내용들이 많으니 그냥 공부하도록 하자.

<br/>
[사전지식] 확률론, 해석학, 미분적분학

## Reference
- [Introduction to Mathematical Statistics, Robert V. Hogg.](https://minerva.it.manchester.ac.uk/~saralees/statbook2.pdf)

# Contents
1. [Probabilty and Distributions](#1-probabilty-and-Distributions)
- MGF
- Characteristic Fucntion
- Inequalities(마코브 부등식, 코쉬 슈바르츠 부등식, 엔트로피와 섀넌 부등식)
2. [Multivariate Distributions](#2-multivariate-Distributions)
- Joint Probability
- Marginal Distribution
- Transformation
- Conditional Distribution
- Independence
- Correlation Coefficient
3. Some Special Distributions
4. Some Elementary Statistical Inference
5. Consistency and Limiting Distributions
6. Maximum Likelihood Methods
7. Sufficiency
8. Optimal Tests of Hypotheses
9. Inferences About Normal Linear Models
10. Nonparameteric Robust Statistics
11. Bayesian Statistics 

---

# 1. Probabilty and Distributions
## 1.9. Some Special Expectations

이산확률변수 X의 기대값에 대한 정의는 다음과 같다. 
<center> $ E(X) = \sum_x x p(x) $ </center>

### 적률생성함수
<br/>
분포의 가장 중요한 성질 중 하나는 적률생성함수(MGF, Moment Genrating Function)다. 그 이유는 분포와 MGF는 unique하게 1:1 대응이기 때문이다. 즉, 특정 확률변수의 MGF를 알면 역으로 분포를 알 수 있다. 또한 분포의 여러가지 통계량을 구하거나 증명을 할 때 mgf가 유용하게 이용된다.
<br/>  
[적률생성함수 정의]
> <center>$ M(t)=E( e^{tX} )  = \sum_x e^{tX} p(x) $ </center>

위에서 언급했듯이 분포와 MGF는 1:1 대응이다. 따라서 두 확률변수 $ X $, $ Y $가 동일한 확률분포를 가지면 두 확률변수의 MGF 역시 동일하다.
> 성질1. $ F_X(z) = F_Y(z)$ for all $ z \in \mathbb{R} $ iff $ M_X(t) = M_Y(t) $ for all $ t \in (-h,h) $ for some $h>0$  

MGF의 이름처럼 $ M_X(t) $ 를 m차 미분하면 확률변수의 m차 moment허(확률변수의 m차 기대값)를 생성할 수 있다. 
MGF를 구할 때 exponential 함수는 미분꼴이 자기자신과 같고, $e^0=1$이라는 성질을 이용한다. 
  
> 성질2. $ E(X^m) = \sum_x x^m p(x) $ or $ \int_{-\infty}^{\infty} x^m f(x) $ 
  
- 1차 미분 <br/>  
$ M'(t) = \frac{M(t)}{dt} = \sum_x xe^{tx}p(x) $ or $ \int_{-\infty}^{\infty} x e^{tx}f(x) dx $ <br/>
여기에 $ t=0 $ 을 대입하면 $ M'(0) = E(X) =\mu $
  
  
- 2차 미분 <br/>
$ {M}^{''}(t) = \sum_x x^2 e^{tx}p(x) $or $ \int_{-\infty}^{\infty} x^2 e^{tx}f(x) dx $ <br/>
여기에 $ t=0 $ 을 대입하면 $ {M}''(0) = E(X^2) $ 
  
- 분산 <br/>
위에서 구한 1차, 2차 적률을 이용해 분산을 구할 수 있다. $ \sigma^2 = E(X^2)-E(X) = {}^{''}(0) - M'(0) $
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
  $ ( M_{ X^* } (0) + \frac{ t }{\sigma \sqrt{n} } M_{ X^* }'(0) + \frac{ t^2 }{ 2 n \sigma^2 }  {M}''_{ X^* } (0) )^n $ <br/>
  $ = ( 1+ \frac{t^2}{2 n \sigma^2 } \sigma^2  )^n $, 
  <br/>
  since
  
  - $ M_{X^*}(0) = E(e^0)=1 $ 
  - $ M_{X^*}'(0) = \mu = 0 $ 
  - $ {M}^{''}_{X^*}(0) = \sigma^2 $ 
  
  $ =  ( 1+ \frac{t^2}{2 n }  )^n  \rightarrow e^{t^2/2}  $ as $ n  \rightarrow \infty $ 
  
  마지막에 도출된 $ e^{t^2/2} $ 이 $ N(0,1) $ 의 MGF다.

  
### 특성함수
확률분포에 따라 MGF가 존재하지 않는 경우가 있다. 어떤 분포에 대해서도 특성함수(CF, charateristic function)는 존재하기 때문에 MGF가 존재하지 않을 경우 CF를 사용한다. 
<br/>  
[특성함수의 정의]
> <center>$ \phi(t) = E( e^{itX} )  = \sum_x e^{itX} p(x) $ <center/>

적률생성함수에서 설명한 성질들이 특성함수에도 동일하게 적용된다
1. $ E( e^{it S_n} ) = \phi(t)^n $
2. CLT 증명 가능
3. m차 모먼트가 유한하다면, $ \phi^{(m)}(t) = E( \partial^m e^{it X_1} / \partial t^m ) = E( (i X_i )^m e^{it X_1} )  $ <br/>
  $ \phi^{(m)}(0) = i^m E(X_1^m) $ <br/>

## 1.10. Important Inequalities
기대값, 확률에 관한 부등식을 다룬다. 부등식을 통해 특정 조건에서의 대소관계와 상한 등의 법칙을 알 수 있다. 
또한 부등식의 대소관계와 극한을 이용하면 theorem의 증명과정에 이용할 수 있다. 
예를 들면 (3)체비셰프 부등식은 대수의 법칙(Law of Large Number)을 증명하는 데 사용된다.
  
(1) Jensen's Inequality: 아래로 볼록(convex) 함수와 평균
<img class="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c7/ConvexFunction.svg/1280px-ConvexFunction.svg.png"  width=550px/>
> <center> If $ \phi $  (strictly) convex function, then $ \phi( E(X) ) \geq E(\phi(X)) $ <center/> 

어떤 함수가 convex(아래로 볼록)일 때 함수값의 평균이 평균의 함수값보다 크거나 같다. 이미지를 보면 젠센 부등식을 쉽게 이해할 수 있는데 함수값의 평균은 핑크색 선 위에 있고 평균의 함수값은 검정색 선 위에 있는데 두 점 사이의 구간에서 항상 핑크색 선이 검정색 선보다 위에 있다.
위 부등식의 증명은 [강의노트](https://www.cs.purdue.edu/homes/hmaji/teaching/Spring%202018/lectures/note01.pdf) p5-p6에서 설명한다.
$ \phi(x) $ 를 $ X=\mu $ 에 대해 2차 테일러 전개를 하여 부등식을 증명한다.

(2) Markov Inequality: 평균으로 구하는 누적분포함수확률의 상한  
> <center> If $ h(X) \geq 0 $ and $ k>0 $, then $ P(h(x)>k) \leq E(h(X))/k $ <center/>   

대부분의 증명은 $ h(X)=X $ 의 경우에 대해 서술하지만 $ h(X) $ 가 양의 값의 범위의 함수라면 $ h(X)=X $ 의 경우와 동일하게 부등식이 성립된다.
증명은 [여기](https://www.probabilitycourse.com/chapter6/6_2_2_markov_chebyshev_inequalities.php)를 참조한다.
  
(3) Chebyschev Inequality: 표준화된 확률변수의 누적분포함수확률 상한
> <center> If $ E(X)=\mu $, $ Var(X) =\sigma^2 < \infty $ and $ k>0 $, <center/>
> <center> then $ P(|X-\mu| \geq k \sigma) \leq 1/k^2 $  or  $ P(|X-\mu| < k \sigma) \geq 1-1/k^2 $ <center/>

분산 역시 기대값의 일종이다.( $Var(X) = E((X-\mu)^2) $ ) 분산을 구할 때 $ \mu $ 를 기준으로 적분구간을 나눠( $ (-\infty, \mu) $, $ (\mu, \infty) $ ) 한쪽 적분 구간을 없애 부등식을 만들면 체비셰프 부등식을 증명할 수 있다. 증명은 [여기](https://zhengtianyu.wordpress.com/2014/01/04/proof-of-chebyshevs-inequality/)를 참조한다.
  
(4) Cauchy-Schwarz Inequality: 기대값의 곱과 곱의 기대값 대소비교
> <center> $ E(XY)^2 \leq E(X^2) E(Y^2) $ <center/>
> <center> = holds iff $ Y = \lambda X $ for some $ \lambda \in \mathbb{R}  $ <center/>
  
증명은 [여기](https://www.probabilitycourse.com/chapter6/6_2_4_cauchy_schwarz.php)를 참조한다. <br/>
위 부등식에 의해 상관계수의 절대값이 언제나 1 이하임을 밝힐 수 있다. <br/>
$ X^* = \frac{X-E(X)}{\sqrt{var(X)}} $ <br/>
$ Y^* = \frac{Y-E(Y)}{\sqrt{var(Y)}} $ <br/>
$ (corr(X,Y))^2 = ( E( X^* Y^* ) )^2 $ <br/>
$ \leq E( X^{ *2 } ) E( Y^{ *2 } )  = 1 \cdot 1 = 1 $
  
(5) Shannon Inequality
  
섀넌 부등식을 설명하기에 앞서 정보이론의 엔트로피 개념을 이해해야 한다. 엔트로피는 정보부족으로 인한 불확실성을 의미한다. 정보에 대한 확신(로그 확률 밀도)에 마이너스 부호를 취한 것의 기대값을 엔트로피라고 생각하면 된다. 따라서 deterministic(확정적)일수록 엔트로피가 낮다. 정보 엔트로피의 정의는 다음과 같다. <br/>
 
<font size="3"> **Shannon Entropy** </font> <br/>  
> <center> $ H(x) = E( - ln f(x) ) = - \int f(x) ln f(x) $  <center/>
 
간단한 예로 동전을 던지는 행위를 X라 하고 동전이 앞면이 나오는 사건을 X=1이라고 하자. 동전이 앞면이 나올 확률(x축)에 따른 엔트로피(y축)를 그리면 다음과 같다. 상식과 부합하게 앞면이 나올 확률과 뒷면이 나올 확률이 반반(P(X=1)=0.5)일 때 가장 높은 불확실성, 즉 엔트로피를 갖는다.

- $ p=0.5 $ 일 때 엔트로피는 <br/>
-P(앞면이 나올 확률) $ \log_2 $ P(앞면이 나올 확률) - P(뒷면이 나올 확률) $ \log_2 $ P(뒷면이 나올 확률) <br/>
=  $ -p \log_2 p - (1-p) \log_2 (1-p) $ <br/>
=  $ -0.5 \log_2 0.5 -0.5 \log_2 0.5  = 1 $

<img class="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/22/Binary_entropy_plot.svg/800px-Binary_entropy_plot.svg.png" width=300px/>
<br/>


<font size="3"> **Shannon Inequality** </font> <br/>  
> <center> Let $ p(.) $ , $ q(.) $ p.d.f. of $ X \in \mathbb{R} $ & $ E_p(h(x)) = \int h(x) p(x) d\mu $  <center/>
> <center> Then, $ E_p( ln p(x) ) \geq E_p( ln q(x)) $ <center/>
> <center> = holds iff $ p(x) = q(x) $ for all $ x $ <center/>
  
다시 섀넌 부등식을 설명하면, 섀넌 부등식은 참 분포가 $ p $ 일 때 분포 $ p $ 에서의 로그기대값의은 어떤 분포 $ q $ 의 로그기대값보다 크거나 같다는 내용의 부등식이다. 
적절한 증명이 없어 다음 증명은 소병수 교수님의 수업을 참고했다. <br/>


<font size="3"> **Proof. Shannon Inequality** </font> <br/>
Let $ R = q(x)/p(x) $ , $ \phi(r) = -ln r $ ,  $ r>0 $ . Then $ \phi '' (r) = 1/ r2  > 0 $ <br/>
여기서 참 확률 분포는 $ p(x) $ , 후보 확률 분포는  $ q(x) $ 라 하자.
$ \phi(r) $ 은 2차 미분값이 항상 양수이므로 strictly convex다. 따라서 (1) 젠센부등식을 대입할 수 있다. <br/>
젠센 부등식에 의해 <br/>
$ E_p(\phi(R)) \geq \phi(E_p(R)) = -ln(E_p(R)) = -ln1 = 0 $  <br/>
 since $ E_p(R) = \int \frac{q(x)}{p(x)} p(x)  d\mu = \int q(x) d\mu = 1 $ (실수 전 구간에 대해 확률분포를 적분 1) <br/>
 $ \therefore  E_p(\phi(R)) = E(-ln (q(x)/p(x))) \geq 0 $ <br/>
  $ E(ln (p(x)) \geq E(ln (q(x)) $ <br/>

<font size="3"> **Kullback Leibler Divergence** </font>  <br/>
KL 발산은 엔트로피 개념을 이용해 동일 확률변수 X에 대해 분포 간 유사성을 측정한 것이다. <br/>
참 확률분포가 $ p(x) $ 일 때 $ p(x) $ 와 어떤 확률분포 $ q(x) $ 의 거리를 쿨백발산으로 나타내면 다음과 같다. $ p(x) = q(x) $ 이면 쿨백 라이블러 발산이 0이다. 
> <center> $ D_{KL} (p | q) = E_p( ln p(x) - ln q(x) ) = \int p(x) ln \frac{p(x)}{q(x)} d\mu $ <center/>    


---

# 2. Multivariate Distributions

## 2.1. Joint Probability Distriubution

결합확률분포란 두 개 이상의 변수를 동시에 고려하는 확률분포를 일컫는다. 결합 확률 분포 역시 확률 이므로 [확률의 공리](https://en.wikipedia.org/wiki/Probability_axioms)를 따른다. 이 챕터에서는 적분이 많으니 사전지식을 갖추길 바란다. <br/><br/>

<font size="3"> **결합확률분포가 중요한 이유는 무엇일까?** </font>  <br/>
- 결합확률분포를 알면 두 변수 간 관계 뿐 아니라 한 변수만의 성질(marginal distriubtion), 한 변수가 주어졌을 때 다른 변수의 분포(conditional probability)를 모두 알 수 있기 때문이다. 


<font size="3"> **정의** </font>  <br/>

- 이산 확률 변수의 결합 확률 분포 <br/>
$ p_{X_1, X_2}(x_1, x_2) = P(X_1 = x_1, X_2 = x_2) $

- 연속 확률 변수의 누적 결합 확률 분포 <br/>
$ F_{X_1, X_2}(x_1,x_2) = P[X_1 < x_1, X_2<x_2] $ <br/>
$ P[a_1<X_1<a_2, b_1<X_2<b_2] = F_{X_1, X_2}(b_1, b_2) - F_{X_1, X_2}(a_1, b_2) - F_{X_1, X_2}(b_1, a_2) + F_{X_1, X_2}(a_1, a_1)$ <br/>
-> 구간에 대한 적분은 두 축을 그려 각 구간을 빼고 더해보면 쉽게 구할 수 있다.  

eg. 두 배터리의 수명이 확률 분포 $ X $ , $ Y $ 이며 서로 연결되어 함수 $ f $ 의 관계를 가진다고 가정하자. <br/>
- $ f(x,y) = 4xy e^{-(x^2+y^2)} $ at $x>0, y>0$, 0 otherwise <br/>
- 이 때 두 배터리의 수명이 모두 $\sqrt (2) /2 $ 이상일 확률은 다음과 같다. <br/>
$ P(X>\sqrt 2 /2, Y>\sqrt 2 /2)  $ <br/>
$= \int^{\infty}_{\sqrt 2 /2}  \int^{\infty}_{\sqrt 2 /2} 4xy e^{-(x^2+y^2)} dx dy $<br/>
 $ = $  적분 과정생략 $ = (e^{-1/2})^2 $