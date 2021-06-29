---
title: Computational Statistics_Optimization
category: Theory
order: 2
use_math: true
comments: true
toc: true
toc_sticky: true
toc_label: 목차
---

### Reference
- Computational Statistics, Wiley Series
- Wikipidia

# Contents
1. Meaning of Optimization
2. Optimization of Nonlinear Equations
	- Continuous function, computational method
	- Problem is solved by target function itself.
	- Eg. Newton's Method, Taylor Expansion, Descent Method
3. Combinatorial Optimization
	- Discrete and complex function, computational method
	- Problem is solved after input is iteratively put into target function.
	- Eg. Local Search, Gene Algorithm, Simulated Annealing,
4. EM Algorithm
	- Statistical method
	- One of the moethds of MI(Multiple Imputation) of missing value
	
# 1. Meaning of Optimization
## 1.1. 최적화의 개념

*"Mathematical optimization or mathematical programming is the selection of a best element from some set of available alternatives."* -  wikipedia

최적화란 Input element 중 최적의 조합을 찾는 것이다. 풀어 설명하자면, 탐색 가능한 input의 set  안에서 주어진 함수를 극대화 혹은 극소화 할 수 있는 최적의 input을 찾는 것이다. 예컨대, 우도함수를 '최대화'하는 모수인 MLE를 '찾는 것', 딥러닝에서 Loss 함수를 '최소화'하는 가중치를 '찾는 것' 모두 최적화의 일종이다. Usecase로는 수율을 모델링한 함수에 대해 수율을 극대화하는 Recipe 조합(input)을 찾는 것 또한 최적화의 일종이다.

최적화는 주로 어떤 함수를 극대화 또는 극소화할 때 사용된다. 간단한 예로 연속함수를 극대화하는 것을 수식으로 표현하자면 다음과 같다.  
<center>$$ \text{Find } x^{*} \text{ such that } f(x^{*}) &geq; f(x) \text{ for all } x $$ </center>  
<center>$$ \text{where } |x*-x| <&delta;, &delta;>0 $$</center>  

## 1.2. 최적화에 대한 접근방식
최적화 대상이 되는 함수의 성질, 제약사항 등에 따라 다른 최적화 방법을 사용해야 한다. 
목차에 기술한대로 input의 값이 연속인지, 이산인지 혹은 미분이나 근사법(approximaiton)으로 
해를 구할 수 있는 목적함수인지 등에 따라 다른 방법으로 최적화를 해야한다.

# 2.  Optimization of Nonlinear Equations
등식을 통해 해를 구할 수 있는 경우에 해당한다. 구체적으로, 식의 미분을 통해 최적값을 구한다.
위에서 언급한 MLE 역시 우도함수를 미분해 0이되는 모수를 찾은 예이므로 이 방식에 속한다. 
하지만 미분을 통해 해를 구할 때에는 해의 유일성, 근사적으로 구한 해의 수렴성, 근사적으로 
구할 때의 값의 초기값 등 고려할 것들이 많다.

## 2.1. 단변량(Univariate)에서의 최적화
미분을 통해 최적값을 구한 책의 예를 들어보겠다.  

<center>$$ g(x) = \frac{log(x)}{1+x} $$</center>    
<center>$$ g'(x)=\frac{1+1/x-log(x)}{(1+x)^{2}} $$</center>    
<center>$$ x^{*}\approx 3.591 $$</center>  

 
### 2.1.1. Bisection Method

위 예제의 $ x^{*} $ 는 정의역 구간을 좁혀나가며 점근적으로 구할 수 있다. 
1차 미분 $ g' $ 이 구간 $ [a_{0},b_{0}] $ 에서 연속일 때 
$ g'(a_{0}) g'(b_{0}) \leq 0 $ 이라면 
$ [a_{0},b_{0}] $
사이에 해가 최소 한 개 이상 존재한다.
해를 찾는 것을 반복할수록 탐색구간이 좁아지므로 
$ [a_{0},b_{0}] \supset [a_{1},b_{1}] \supset [a_{2},b_{2}] \supset \text{...} $
와 같이 된다.


**[Updating Rule]** <br/>
- Let initial value $ x^{0}=(a_{0}+b_{0})/2 $ ,  
then, $ [a_{t+1},b_{t+1}] $ 
equals to $ [a_{t},x_{t}] $ , <br/>
if $ g'(a_{t})g'(x_{t+1}) \leq 0 $
equals to $ [ x_{t},b_{t}] $ , <br/>
if $ g'(a_{t})g'(x_{t+1})>0 $
where $ x^{t+1}=(a_{t}+b_{t})/2 $

다음 stopping rule에 따라 값이 수렴했다고 간주하고 $ x $ 값의 업데이트를 멈춘다.

**[Stopping Rule]**
- Absolute Convergence criterion <br/>
$ |x_{t+1}-x_{t}| \leq \epsilon $,
$ \epsilon $ 는 매우 작은 0이 아닌 수
- Relative Convergence Criterion <br/>
$ \frac{|x_{t+1}-x_{t}|}{|x_{t}|} \leq \epsilon $ <br/>
단, 분모가 0일수도 있으므로 다음 식으로 대체할 수 있다. <br/>
$ \frac{ |x_{t+1}-x_{t}| }{ |x_{t}| + \epsilon } \leq \epsilon $


수렴 이슈가 발생할 수 있는데, 이 때 $ a_{t+1}=(a_{t}+b_{t})/2 $ 
대신 robust한 방법으로 
$ a_{t+1}=a_{t}+(b_{t}-a_{t})/2 $ 를 사용할 수 있다. 
수렴을 위해 알고리즘을 수정하더라도 수렴에 실패할 수 있다. 
이경우 위 stopping rule에 따라 업데이트를 멈추지 말고, 업데이트 횟수를 N번으로 고정한다.

### 2.1.2. Newton's Method
이 방법은 타겟 함수를 2차 테일러 전개를 하여 최적 근사값을 구하는 방법이다. 

**[Alogrithm]**
<center> $ 0=g'(x^{*}) \approx g'(x^{t})+(x^{*}-x^{t})g''(x^{*}) $ </center>
<center> $ x^{*}=x^{(t)}-\frac{g'(x^{t})}{g''(x^{t})}=x^{(t)}+h^{(t)} $
 where $ h^{(t)}=-\frac{g'(x^{t})}{g''(x^{t})} $ </center>  

hence, <br/>

**[Update]**
<center>$ x^{(t+1)}=x^{(t)}+h^{(t)} $</center>

<br/>
Eg. 앞서 제시한 예제 $ g(x)=\frac{log(x)}{1+x} $ 의 최적값을 Newton's mothod를 통해 구해본다.
$ h^{(t)}=\frac{(x^{(t)}+1)(1+1/x^{(t)}-log(x^{(t)})}{3+4/x^{(t)}+1/(x^{(t)})^2-2log(x^{(t)})} $ 
초기값 $ x^{(0)}=3 $ 으로부터 시작할 때,  $ x^{(4)} \approx 3.59112 $ 으로 4th iteration 근사값이 최적값에 가까워진다.

- 수렴가능 <br/>
목적 함수의 모양과 시작점 설정에 따라 위 방법의 수렴이 결정된다. 
단, $ g' $, $ g'' $ 모두 연속적으로 미분가능하고 convex하며 근을 가질 때 위 방법은 시작점과 관계없이 항상 수렴한다.

- 수렴속도 <br/>
다음은 수렴 속도를 결정하는 수렴 차수 $ \beta $다. 
높은 차수를 가질수록 Newton's Method가 빨리 수렴하지만, 
수렴의 robustness 관점에서 취약하여 근사값의 정확성이 떨어진다. 
(robustness란 어떤 알고리즘이 타당하기 위해 세우는 가정이 깨지더라도 유의한 결과를 내는 것을 일컫는다.)

**[Convergence Order $ \beta $]**
<center>$ \lim_{t\rightarrow \infty } \epsilon{(t)}=0 $ and 
$ \lim_{t\rightarrow \infty }\frac{|\epsilon ^{(t+1)}|}{|\epsilon{(t)}|^\beta}=c $ </center>
<center>for some $ c \neq 0 $ and $ \beta>0 $ </center>

<br/>
**[Fisher Scoring]** <br/>
Newton's Method를 사용하여 MLE를 구하는 것을 일컫는다.  
$ l $을 어떤 분포의 log likelihood라 할 때, 피셔 스코어링을 통한 MLE의 추정은 다음과 같다.
<center> $ \theta^{(t+1)}=\theta^{(t)}+\frac{l'(\theta^{(t+1)})}{I(\theta^{(t)})} $ </center>
여기서 $ I(\theta^{(t)}) $ 는 Fisher Information의 근사값, $ -l''(\theta^{(t)}) $를 의미한다. 

참고. 로그 우도함수를 2차 미분한 것에 기대값을 취하면, 로그 우도함수의 제곱항의 기대값만 남는다.
 즉 로그 우도 함수의 분산의 역수가 피셔의 정보량이다. 이는 데이터를 통해 모수에 대해 얻을 수 있는 정보의 크기를 의미한다.

<br/>
**[Secant Method]** <br/>
Newton's Method에서 2차 미분 계산이 어려울 때, 
1차 미분을 수치미분한 것을 2차 미분식에 대체하여 사용하는 방법이다.
<center> $ x^{(t+1)}=x^{(t)}-g'(x^{(t)})\frac{x^{(t)}-x^{(t-1)}}{g'(x^{(t)})-g'(x^{(t-1)})} $</center> 
단 $ t \geq 1 $ 이며, 두 시작점 $ x^{(0)} $와 $ x^{(1)} $ 이 필요하다.

### 2.1.3. Gradient Descent Method
이 방법은 reference 책에는 없지만, 딥러닝 역전파시 흔히 사용되므로 기술하겠다.
<center>  $ x_{i+1} = x_{i} - \gamma_{i} \nabla f(x_{i}) $ </center> 
위와 같은 식으로 최적화를 하는 이유는 기울기의 반대방향으로 input을 이동하여 기울기가 0인 지점을 찾아가기 때문이다.
여기서 $ \gamma_{i} $ 는 learning rate에 해당한다.

## 2.2. 다변량(Multivariate)에서의 최적화
p 차원 벡터 $ x^{(t)}=( x_1^{(t)} , ... , x_p^{(t)} )^T $에 대해 최적화하는 문제를 생각해보자.
단변량 최적화와 마찬가지로 테일러 전개를 이용해 iterative한 계산으로 지역 최적화 값을 구한다. 
이전 iteration 값과 현재 iteration 값의 차이가 작아질 때 최적해를 구하는 iteration을 멈춘다.

**[Convergence]**
<center> $ D(x^{(t+1)},x^{(t)})<\epsilon $ or </center> 
<center> $D(x^{(t+1)},x^{(t)})/D(x^{(t)},0)<\epsilon$ or </center> 
<center> $D(x^{(t+1)}, x^{(t)})/(D(x^{(t)},0)+\epsilon)<\epsilon $ </center> 
<center> where $ D(u,v)=\sum{|u_i-v_i|} $ or $ D(u,v)=\sqrt{\sum{(u_i-v_i)^2}} $ </center> 


### 2.2.1. Newton's Method and  Fisher Scoring 

벡터 버전으로 목적함수 $ g $ 를 테일러 전개하면 다음과 같다.
<center>$ g(x^*)=g(x^{(t)})+(x^*-x^{(t)})^Tg'(x^{(t)})+
\frac{1}{2}(x^*-x^{(t)})^Tg''(x^{(t)})(x^*-x^{(t)}) $ </center>
 
위 식을 $ x^* $ 에 대해 미분하면 다음 식과 같다.
<center>$ g'(x^{(t)})+g''(x^{(t)})(x^*-x^{(t)})=0 $</center>
<center>$ \Leftrightarrow x^{(t+1)}=
x^{(t)}-g''(x^{(t)})^{-1}g(x^{(t)}) $</center>

마찬가지로 p개 모수의 MLE는 다음과 같이 구한다.
<center> $ \theta^{(t+1)}=
\theta^{(t)}+I(\theta^{(t)})^{-1}l'(\theta^{(t)})$ </center>
단변량의 경우와 같이 $ l'(\theta^{(t)}) $는 로그 우도함수의 1차 미분이고, 
$ -I(\theta^{(t)})^{-1} $는 로그우도함수의 2차 미분에 해당한다.


### 2.2.2. Iteratively Reweighted Least Squares

회귀계수를 추정하는 것은 잔차를 최소화하는 일이므로 최적화에 해당한다. 단순회귀모형(OLS)의 모수는 미분을 통해 간단히 구해지는 반면, GLM의 모수는 chain rule을 통해 근사적으로 모수를 추정해야 한다. <br/>
 <br/>
[참고: OLS 모수 추정] <br/>
<center> $ E(y|X) = X^T \beta $  </center>
<center> $ Squared Error = (y - X^T \beta)^T (y - X^T \beta) = y^T y - 2\beta^T X^T y + \beta^TXX^T\beta $  </center>
<center> $ \frac{\partial}{\partial \beta} Squared Error = -2X^Ty + 2 X^TX\beta = 0 $  </center>
<center>  $ \hat{\beta} = (X^T X)^{-1}X^T y $ </center>
<br/>
OLS는 $ E(y|X) $ 를 구하는 게 목적인 반면, GLM은  $ g(E(y_i|x_i)) $ 를 구하는 게 목적이다. ( $ g $ 는 GLM의 link function) 
이 때 IRLS를 통해 GLM의 모수를 추정한다. <br/>
	
<br/>
[GLM 모수추정] <br/>
GLM에서 y의 분포는 대개 지수족(exponential family)이다. GLM 전반에 대해 다루는 것은 최적화 내용의 범위를 넘어가므로 GLM은 회귀분석에서 다루도록 하겠다. <br/>
 <br/>
[참고: 지수족] <br/>
<center> $ f(y|\theta) = \frac{y\theta- b(\theta)} {a(\phi) + c(y,\phi))}  $ </center>
<center> canonical parameter $ \theta $ , dispersion parameter $ \phi $ </center>
<center> $ g(E(y_i|x_i)) = x_i^T \beta = \eta_i $ </center>
	
이 때, 로그 우도함수( $ l $ )를 최대화하는 모수를 추정하려면 chain rule에 의해 다음을 미분해야 한다.
<center> $ \frac{\partial l_i}{\partial \beta_j} = 
	\frac{\partial l_i}{\partial \theta_i}  
	\frac{\partial \theta_i}{\partial M_i} 
	\frac{\partial M_i}{\partial \eta_i}
	\frac{\partial \eta_i }{\partial \beta_j}$ = 0 ...(a)</center>
	
모수 $ \beta $ 를 추정하는 과정을 결론 위주로 설명하겠다.
(a)를 식정리하면 다음과 같다.
<center> $ X^T D V^{-1} (y-M) = 0 $ </center>
- $ D $ : $ \frac{\partial M_i}{ \partial \eta_i} $ 을 대각성분으로 갖는 대각행렬, 단 $ \eta = X \beta $
- $ V  = cov(y) $ 행렬
- $ E(Y) = M $
	
<br/>
Fisher Scoring에 의해 $ \beta^{(t+1)}=\beta^{(t)} + (J^{(t)})^{-1} u^{(t)} $, 단 $ J^{(t)} $는 피셔 정보 행렬<br/>
<center> -> $ J^{(t)} \beta^{(t+1)}= J^{(t)} \beta^{(t)} + u^{(t)} $ ...(b) </center>
(b)에  $ u $ 와 $ J $를 대입한다.
- score function $ u = X^T W D^{-1} (y-M) $
- Information matrix $ J =  X^{T} W X $ 
- 단, W는 $ w_i = \frac{ (\partial M_i/ \partial \eta_i)^2 }{ var(y_i) }$ 를 대각성분으로 갖는 행렬

결론적으로 (c)를 업데이트하며 GLM의 모수를 추정한다.
<center> $ J^{(t)} \beta^{(t+1)} = X^T W^{(t)} Z^{(t)} $ where $ Z^{(t)} = X \beta^{(t)} + (D^{(t)})^{-1} (y-M^{(t)}) $   </center>
<center> -> $ \beta^{(t+1)} = (J^{(t)})^{-1} X^T W^{(t)} Z^{(t)}  = (X^T W^{(t)} X)^{-1} X^T W^{(t)} Z^{(t)} $ ...(c) </center>

<br/>
# 3.   Combinatorial Optimization
등식으로부터 최적화하는 것과 달리, discrete value의 조합(예. 경우의 수)을 통해 
최적값을 구하는 경우에는 3장의 방법을 적용한다.

## 3.1. Hard Problem and NP-Completeness
Hard Problem은 모든 조합의 경우의 수를 확인해야하는 문제로 탐색 영역이 너무 커서 해를 찾기 어렵다. 
대표적인 예로 traveling salesman problem을 생각해볼 수 있다. 
이 문제는 salesman이 p개의 도시를 거쳐 원점으로 돌아올 때 최소 경로를 찾는 문제다. 
방향을 고려하지 않을 때 $ (p-1)!/2 $개의 후보 경로가 존재한다.

Hard Problem에서 계산 횟수는 input p에 관한 다항식(예. $ p^2 $, $ p! $ 등) 
$ h(p) $ 에 대하여 $ \mathcal{O}(h(p)) $ 에 제한(bound)된다. 
참고로 $ \mathcal{O} $ notation은 big o를 찾아보길 바란다.


Problem complexity 이론에서는 두 가지 이슈를 주로 다룬다. 
첫번째는 최적화(i.e. 탐색), 두번째 decision(i.e. recognition)이다. 
즉, 어떤 목적함수를 최적화할것이며 탐색영역을 얼마나 넓게 설정할 것인가가 주요 화두다.


NP problem을 설명하기 전, 몇 가지 개념들을 소개한다.
- A **decision problem** is a yes-or-no question on an infinite set of inputs.
- **NP(nondeterministic polynomial time)** is a complexity class used to classify decision problems. NP is the set of decision problems for which the problem instances, where the answer is "yes", have proofs verifiable in polynomial time by a deterministic Turing machine.
- A **Turing machine** is a general example of a central processing unit(CPU) that controls all data manipulation done by a computer, with the canonical machine using sequential memory to store data.

- $P$:  다항식으로 이루어진 계산 횟수(polynomial time, eg.$ \mathcal{O}(p^k) $, 
단 $ p $개의 입력값과 상수 $ k $) 안에 해결되는 문제는 $ P $ 클래스에 속한다. 
이 경우 해를 효율적으로 구하는 것이 가능하다.

- $NP$: polynomial time 안에 해결될 수 있는지 체크해볼만한 decision problem을 
NP problem이라 부른다. 즉 모든 decision problem의 set은 NP problem이고, $ P $는 $ NP $에 속한다.

NP-complete 문제와 NP-hard 문제 개념을 도식화하면 다음과 같다.      

<br/>

<img class="center" 
src="https://upload.wikimedia.org/wikipedia/commons/a/a0/P_np_np-complete_np-hard.svg" 
width=550px/>        

<br/>

- $NP- Hard$
- $NP- Completeness$


## 3.1. 예제
- 예제1. 유전자 매핑
- 예제2. 회귀분석에서의 변수 선택
총 $p$개의 설명변수 후보가 존재할 때, AIC를 최소화하는 $s$개의 설명변수를 선택하여 
best model을 구한다.  AIC는 회귀식의 예측 성능을 나타내는 RSS항과 모델의 복잡성에 대한 
패널티를 나타내는 두 개의 항의 합이다. 
$ AIC = N log{RSS/N} + 2(s+2) $ 이 경우 상수항을 포함하여 각 계수를 포함하거나 제외하므로 
subset model은 $ 2^{p+1} $ 개의 후보 모델이가 존재한다. 
단, 베이지안 회귀의 경우 사후모델의 확률을 최대화하는 subset model을 best model로 삼는다.
<br/>

**Heurisitic?**
- 감내 가능한 시간 내 지역 최적화 값을 찾는 것을 휴리스틱이라한다.
- 전역 최적화 값의 후보값을 찾는다.


**휴리스틱의 특성** <br/>
< Local Search > <br/>
(1) 현재 후보 해의  iterative improvement <br/>
(2) 특정 iteration에서 지역 이웃값들 탐색에 대한 한계 <br/>

**휴리스틱 이외 조합 최적화 알고리즘 리스트**
- Simulated Annealing
- Genetic Alogrithms
- Tabu Algorithms


## 3.2. Local Search
- 기본 개념: 현재 후보 최적값 $ \theta^{(t)} $ 의 이웃값 $ \mathcal{N}(\theta^{(t)}) $ 로부터 
다음 후보 최적값 $ \theta^{(t+1)} $ 을 iterative하게 구한다. 
- 장점: 전역 탐색 구간 중 작은 일부분만을 탐색한다.
- 단점: 전역 최적값이 아닌 지역 최적값을 구할 가능성이 높다.
- *k-optimization, random starts local search*

[후보 탐색 방식]
- $ k-change $ : 현재 최적값 후보의 이웃 중 k개를 다음 최적값 후보로 삼는 것
    - 예.  회귀모형 best subset 모형 구할 때 변수를 k개씩 넣거나 없애보기
- $ k-optimal $ : *steepest ascent algorithm*(이전 후보값보다 큰 값 중에 후보값을 찾는 것)에 k개의 이웃 후보를 넣어 다음 후보를 찾는 것
- *greedy algorithm*: 미래의 연속적 결과에 관계없이 현 상황에서 best 해결책을 찾는 것
    - 참고. 이웃 후보값을 찾는 방법으로 7장의 *Markov chains* 이용 가능

- *Ascent Algorithm*은 종종 전역 최적값이 아닌 지역 최적값으로 수렴한다. 
이에 대한 해결방법 중 하나가 *Random starts local search*다. 
가장 쉬운 방법으로 후보값의 set인 $ \Theta $에서 *uniform* 분포로부터 시작점을 
독립적으로 여러번 랜덤하게 추출한다. 

[예제] <br/>
(Baseball Salaries) best subset 회귀모델 찾기 <br/>
후보 변수 27개 중 AIC를 최소화하는 모델을 찾기 <br/>
전체 모델 개수는 $ 2^{27} $개


## 3.3. Simulated Annealing
- 기본 개념: 최적값 탐색폭을 담금질 시 식어가는 온도로 빗댄 알고리즘이다. 매 iteration마다 점점 줄어든 폭으로 주변 값을 탐색하며, 새로운 후보값의 확률밀도와 기존 값의 확률밀도에 관한 값(p)을 확률로 후보값 업데이트를 랜덤하게 결정한다. 후보값 채택확률 역시 특정 iteration 구간마다 줄어들어 값을 수렴시키려 한다. 채택확률의 식인 (2)의 $ f $는 열에너지의 감소를 나타내는 Boltzmann 확률($ exp(\Delta E / k \tau)  $)을 사용한다.
	
iteration 횟수를 $ t $, 초기값을 $ \theta{(0)} $, 온도를 $ \tau_0 $, j번째 iteration 블럭을 $ m_j $라 하자. <br/>
(1) 제안분포(proposal density) $ g^{(t)}(.|\theta(t)) $  로부터 $ \theta^{(t)}$의 주변값 $ \mathcal{N} (\theta^{(t)}) $ 내에서 새로운 후보값 $ \theta^* $ 를 뽑는다. <br/>
(2) $ min(1, exp(\frac{ f(\theta^{(t)}) - f(\theta^*) }{ \tau_j })) $ 의 확률로 값을 업데이트한다. ( $ \theta^{(t+1)} = \theta{(t)} $ ) <br/>
(3) 앞서 시행한 (1), (2)를 $ m_j $ 번 반복한다. <br/>
(4) j 배수의 iteration마다 $ \tau_j = \alpha(\tau_j -1) $ , $ m_j = \beta (m_{j-1}) $ 로 업데이트한다.  <br/>

단변량 변수로 목적함수를 최적화하는 담금질 알고리즘을 파이썬으로 코드화하면 다음과 같다. [참고](https://machinelearningmastery.com/simulated-annealing-from-scratch-in- python/)
- obejctive: 최적화할 대상인 목적함수
- bounds: input 탐색 구간
- n_iterations: 탐색 횟수
- step_size: 업데이트 후보가 될 주변값 탐색 폭
- temp: 채택확률에 영향을 미치는 온도값(실제 온도가 아니고 담금질 기법에서의 업데이트 가능성을 의미, temp가 작으면 업데이트 후보의 채택확률이 낮아짐)
	
```
# simulated annealing algorithm
def simulated_annealing(objective, bounds, n_iterations, step_size, temp):
	# generate an initial point
	best = bounds[:, 0] + rand(len(bounds)) * (bounds[:, 1] - bounds[:, 0])
	# evaluate the initial point
	best_eval = objective(best)
	# current working solution
	curr, curr_eval = best, best_eval
	# run the algorithm
	for i in range(n_iterations):
		# take a step
		candidate = curr + randn(len(bounds)) * step_size
		# evaluate candidate point
		candidate_eval = objective(candidate)
		# check for new best solution
		if candidate_eval < best_eval:
			# store new best point
			best, best_eval = candidate, candidate_eval
			# report progress
			print('>%d f(%s) = %.5f' % (i, best, best_eval))
		# difference between candidate and current point evaluation
		diff = candidate_eval - curr_eval
		# calculate temperature for current epoch
		t = temp / float(i + 1)
		# calculate metropolis acceptance criterion
		metropolis = exp(-diff / t)
		# check if we should keep the new point
		if diff < 0 or rand() < metropolis:
			# store the new current point
			curr, curr_eval = candidate, candidate_eval
	return [best, best_eval]	
```

## 3.4. Gene Algotithms		
- 기본 개념: 유전자 알고리즘은 다윈의 자연선택설을 모방한 알고리즘이다. 이전 iteration의 후보값을 부모라고 여길 때, 두 부모 후보값의 교배를 통해 다음 세대의 후보값을 생성한다. 후보 탐색시 적합성이 높은 값의 염 다음 세대로 통과할 가능성이 크고, 적합성이 낮은 값은 후보탐색의 다양성을 높인다.
- [코드 참조](https://machinelearningmastery.com/simple-genetic-algorithm-from-scratch-in-python/)						   
<br/>						   
- genotype
- phenotype: genotype이 발현된 것
- $ C $ : 염색체 element
- $ P $ : 세대의 크기( 대략 $ 2C < P <20C $ )
- genetic operator: 
	- 교차(crossover): 한 세대의 두 후보값(부모)를 조합(breeding)을 통해 다음 세대의 해를 생성 <br/>
	eg. 부모 100110001 & 110100110 -> 자손 110110001 (세 번째, 네 번째 사이를 쪼개 붙임)
	- 돌연변이(mutation): 부모의 공통된 특성을 물려받지 않음, 돌연변이는 breeding 이후 발생 <br/>
	eg. 부모의 세번째 염색체가 모두 0, xx0xxxxxx -> 자손의 세 번째 염색체가 0이 아님 10'1'100110
- Fitness: 후보 최적값을 목적함수에 넣었을 때의 아웃풋 값, 후보값을 넣었을 때 목적함수가 최소 또는 최대값으로 다가가는지 score에 해당
- Selection: 다음 세대 후보를 선택하는 가장 흔한 방식은 토너먼트 선택, 후보값을 k개의 소그룹으로 나눈 후 각 그룹에서 최적의 후보가 부모로 선택된다. 각 부모를 랜덤하게 교배하여 다음 세대를 생성한다. generation gap $ G $ 는 자손에 의해 대체되는 세대의 비율이다. $ G =1/P $ 일 때, 평균 이하의 적합성을 가진 염색체를 자손들이 대체할 수 있다. 
- Permutation Chromosome: 후보값을 더 섞어서 뽑기 위해 1개가 아닌 다중 포인트에서 교차를 시도한다. <br/>
	eg. 752631948 & 912386754 -> 752/386754 & 912/631948 대립형질이 중복되므로 부적절한 순열 염색체다. <br/>
	eg. 752631948 & 912386754 -> xx238x754 61/238/9/754 

## 3.5. Tabu Algorithms
<br/>
	
# 4. EM algorithm
EM 알고리즘은 관찰값이 주어질 때 조건부 분포로부터 결측치를 추측하는 것을 발상으로 만들어진 알고리즘이다. <br/>
- 데이터쌍(complete data): $ Y=(X, Z) $
- 확률변수 $ X $ 로부터 관측 데이터 발생
- 확률변수 $ Z $ 로부터 관측되지 않은 데이터 발생
		
\newcommand{\argmax}{\operatornamewithlimits{argmax}}
\newcommand{\argmin}{\operatornamewithlimits{argmin}}
\DeclareMathOperator{\col}{col}
	
EM 알고리즘은 다음 순서에 따라 동작한다. <br/>
1. E-step: $ Q(\theta|\theta^{(t)}) $ 계산 <br/>
M step에서 구한 $ \theta $ 이용, $ \theta $ 와 관측된 데이터 $ x $가 픽스되었다고 여기고 관측되지 않은 $ Z $ 의 확률밀도함수를 이용해 기대값 계산 
(Z는 integrate out되므로 기대값 구하면 사라짐)
2. M-step: $ \theta^{(t+1)} = \argmax_{\theta}  Q(\theta|\theta^{(t)}) $
3. stopping rule을 충족할 때까지 E-step, M-step 반복
	
- $ Q(\theta|\theta^{(t)} = E ( log L(\theta|Y) | x, \theta^{(t)} ) $ -> 관측된 값 $ x $ 와 이전 iteration으로부터 구해진 $ \theta $ 를 조건부로 넣음 <br/>

