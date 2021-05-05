---
title: Optimization
category: Computational Statistics
order: 2
use_math: true
comments: true
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

**Convergence Order $\beta$**
<center>$ \lim_{t\rightarrow \infty } \epsilon{(t)}=0 $ and </center>
<center>$ \lim_{t\rightarrow \infty }\frac{|\epsilon ^{(t+1)}|}{|\epsilon{(t)}|^\beta}=c $ </center>
<center>for some $ c \neq 0 $ and $ \beta>0 $ </center>