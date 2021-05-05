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
<center>$$ \text{Find} x^{*} \text{such that} f(x^{*}) &geq; f(x) \text{for all} x $$ </center>  
<center>$$ \text{where} |x*-x| <&delta;, &delta;>0 $$</center>  

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
<center>$$ x*\approx 3.591 $$</center>  

