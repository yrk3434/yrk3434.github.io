
# Mathematical Statistics
- 통계학에서 근본이 되는 몇 가지 성질과 theorem을 다룬다.
- 이 포스팅에서는 확률론, 회귀분석 등 주요 과목과 중복되는 내용은 제외하고, 여러가지 수리적 
성질에 관한 내용만 다룬다.

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
<center> $ E(X) = \sum_x x p(x) $ <center/>

<br/>
분포의 가장 중요한 성질 중 하나는 적률생성함수(mgf, Moment Genrating Function)와 특성함수(Charateristic Function)이다. 그 이유는 분포와 적률생성함수, 특성함수는 unique하게 1:1 대응이기 때문이다. 즉, 특정 확률변수의 mgf를 알면 역으로 분포를 알 수 있다.

> Moment Generating Function
> $ M(t)=E( e^{tX} )  = \sum_x e^{tX} p(x) $

