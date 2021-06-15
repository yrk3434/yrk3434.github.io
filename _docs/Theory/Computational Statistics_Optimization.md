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
- NP vs P 다이어그램 이미지: [https://en.wikipedia.org/wiki/NP-completeness](https://en.wikipedia.org/wiki/NP-completeness){:target="_blank"}

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
등식을 통해 해를 구할 수 있는 경우에 해당한다. 널리 사용되는 방법은 미분이다. 
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
단, $ g' $, $ g'' $ 모두 연속적으로 미분가능하고,  
convex하며 근을 가질 때 위 방법은 시작점과 관계없이 항상 수렴한다.

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
p 차원 벡터 $x^{(t)}=(x^{(t)}_{1},...,x^{(t)}_{p})^T$에 대해 최적화하는 문제를 생각해보자.
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
 
위 식을 $ x^{(t)} $ 에 대해 미분하면 다음 식과 같다.
<center>$ g'(x^{(t)})+g''(x^{(t)})(x^*-x^{(t)})=0 $</center>
<center>$ \Leftrightarrow x^{(t+1)}=
x^{(t)}-g''(x^{(t)})^{-1}g(x^{(t)}) $</center>

마찬가지로 p개 모수의 MLE는 다음과 같이 구한다.
<center> $ \theta^{(t+1)}=
\theta^{(t)}+I(\theta^{(t)})^{-1}l'(\theta^{(t)})$ </center>
단변량의 경우와 같이 $ l'(\theta^{(t)}) $는 로그 우도함수의 1차 미분이고, 
$ -I(\theta^{(t)})^{-1} $는 로그우도함수의 2차 미분에 해당한다.

# 3.   Combinatorial Optimization
통계적 수식으로부터 최적화하는 것과 달리, discrete value의 조합(예. 경우의 수)을 통해 
최적값을 구하는 경우에는 2장의 방법을 사용할 수 없다.

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
- 기본 개념: 담금질 시의 온도를 탐색의 폭으로 빗댄 알고리즘이다. 매 iteration마다 점점 줄어든 폭으로 주변 값을 탐색하며, 새로운 후보값의 확률밀도와 기존 값의 확률밀도에 관한 값(p)을 확률로 후보값 업데이트를 랜덤하게 결정한다. 후보값 채택확률 역시 특정 iteration 구간마다 줄어들어 값을 수렴시키려 한다. 채택확률의 식인 (2)의 $ f $는 열에너지의 감소를 나타내는 Boltzmann 확률을 사용한다.
	
(1) 제안분포(proposal density) $ g^{(t)}(.|\theta(t)) $  로부터 $ \theta^{(t)}$의 주변값 $ \mathpzc{N} (\theta^{(t)}) $ 내에서 새로운 후보값 $ \theta^{*} $ 를 뽑는다.
(2) $ min(1, exp(/frac{ f(\theta^{(t)}) - f(\theta^{*}) }{ \tau_j })) $ 의 확률로 값을 업데이트한다. ( $ \theta^{(t+1)} = \theta{(t)} $ )
(3) 앞서 시행한 (1), (2)를 $ m_j $ 번 반복한다.
(4) j 배수의 iteration마다 $ \tau_j = \alpha(\tau_j -1) $ , $ m_j = \beta (m_{j-1}) $ 로 업데이트한다;.  
