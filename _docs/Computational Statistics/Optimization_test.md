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

