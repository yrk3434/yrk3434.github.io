---
title: Deep Learning Overview
category: Theory
order: 5
use_math: true
comments: true
toc: true
toc_sticky: true
toc_label: 목차
---

# Deep Learning Book_책 정리

[책 다운로드 링크] <br/>

[Deep Learning, Ian Goodfellow et al.](https://github.com/janishar/mit-deep-learning-book-pdf/blob/master/complete-book-pdf/Ian%20Goodfellow%2C%20Yoshua%20Bengio%2C%20Aaron%20Courville%20-%20Deep%20Learning%20(2017%2C%20MIT).pdf)

# 개요

- 딥러닝 프레임워크를 구성하는 요소들의 기본 개념을 이해할 수 있다. 
- 챕터의 순서는 책의 순서를 따르지만, 챕터 내 설명은 재구성하였다.
- 책 내용 밖의 설명은 통계 전공의 지식(확률론, 수리통계학, 계산통계학)을 바탕으로 첨언하였다.


# Contents
[Part1. Deep Networks: Modern Practices]

1. Deep Feedforward Networks
2. Regularization for Deep Learning
3. Optimization for Training Deep
4. Convolutional Networks
5. Sequence Modeling: Recurrent and Recursive Nets

[Part2. Deep Learning Research]

1. Linear Factor Models
2. Autoencoders
3. Representation Learning
4. Structured Probabilistic Models
5. Monte Carlo Methods
6. Confronting the Partition Function
7. Approximate Inference
8. Deep Generative Models

# Part1. Deep Networks: Modern Practices

# 1. Deep Feedforward Networks

## 1.1. 피드포워드 네트워크

- 피드포워드 네트워크의 개념 <br/>
피드포워드 네트워크란 한 방향(**acyclic**, 신경망의 연결이 순환하지 않음)으로 연결된 네트워크다. Multilayer Perceptrons(MLPs)라고도 불리운다. 피드포워드 네트워크는 일종의 합성함수인데 $ f= f^{(3)}(f^{(2)}(f^{(1)}(x))) $ 와 같은 꼴의 식이다. 각 식 $ f^{(1)} $ , $ f^{(2)} $ , $ f^{(3)} $ 을 신경망의 layer라고 보면 된다.
- 피드포워드 네트워크는 일종의 여러 겹의 합성함수기 때문에 최적값을 구하기 위해 미분을 할 때 chain rule이 사용된다. 

- 퍼셉트론의 한계 <br/>
퍼셉트론은 일종의 **이진 분류기**다. 퍼셉트론은 어떤 **선형식** $ w \cdot x + b $ 를 기준으로  $ w \cdot x + b >0 $ 이면  1 아니면 0을 반환하여 이진 분류를 수행한다. 여기서 $ w $ 는 기울기, $ b $ 는 절편이다. 한 층의 퍼셉트론이 갖는 한계는 대표적으로 XOR 문제에서 나타난다. $ x_1 $ , $ x_2 $ 이 1 또는 0을 갖는 논리값이라고 할 때, XOR은 $ x_1 $ , $ x_2 $ 중 하나만 1일 때 1을 반환하고, 나머지의 경우 0을 반환하는 논리회로다. 아래 그림을 보면, $ x_1 $ , $ x_2 $ 이 XOR 논리회로를 거친 후 1이면 초록색 점, 0이면 빨간색 점으로 표시되어 있다. 그런데 두 색깔의 점은
어떤 선형식으로 분류할 수 없다. 즉 한 층의 퍼셉트론은 XOR 문제를 해결할 수 없다.

![Image](https://res.cloudinary.com/practicaldev/image/fetch/s--O_kCr-s2--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/lkli02223oqhlac1jetz.png){: height="200"}

결론: 선형식으로 위와 같이 문제를 해결할 수 없을 때, 여러 층의 퍼셉트론(MLP)을 사용하여 **비선형** 식을 만들어 문제를 해결해야 한다.

## 1.2. Gradient-Based Learning

피드포워드 신경망은 비선형 함수이고 그에 따라 **손실함수는 non-convex**다. 따라서 손실함수의 최소값을 찾으려면, iterative하게 gradient 최적화를 해야하며, 근사적으로 구한 최적값이 전역 최적값이라는 보장이 없다. <br/>
<br/>
참고. 연속적으로 미분 가능하며 아래로 볼록(convexity)인 함수는 전역 최소값이 항상 존재한다. <br/>
<br/>
참고. 이 블로그 포스팅 [Computational Statistics_Optimization](https://yrk3434.github.io/Theory/Computational%20Statistics_Optimization/) ch2. Optimization of Nonlinear Equations를 읽어보자.

### 1.2.1. 손실 함수

손실함수는 실제값과 모델을 통해 추정한 값의 차이로 모델의 성능을 측정하는 함수다. 여기서 '차이'는 값의 차이일수도 있고 확률밀도 상의 차이일수도 있다. 해결하고자 하는 문제가 분류인지 회귀인지에 따라 다른 손실함수를 사용한다.
<br/>

#### 분류문제: 크로스 엔트로피

분류문제는 크로스 엔트로피를 통해 실제값과 추정값의 차이를 측정한다. 두 가지 관점(정보이론, 확률론)으로 크로스엔트로피를 설명할 수 있다.

(1) 관점1. 정보이론의 엔트로피 <br/>

참고. 이 블로그 포스팅 [Mathematical Statistics](https://yrk3434.github.io/Theory/Mathematical%20Statistics/)  Shannon Entropy를 읽어보자.

- 학습을 통해 추정한 모델이 실제 함수를 잘 설명하는 모델이라면 추정한 값의 분포가 참 분포에 가까워져 엔트로피가 낮아진다. 
- 실제 분포를 $ p(x) $ , 학습을 통해 얻은 후보 분포를 $ q(x) $ 라 할 때, $ - ln q(x) $ 의 기대값은 $ - \int p(x) ln q(x) dx $ (연속분포의 경우) 또는  $ - \sum p(x) ln q(x) $ (이산분포의 경우)라 할 수 있다. 이것이 크로스 엔트로피다. 이산분포의 경우가 우리가 딥러닝에서 흔히 보는 크로스 엔트로피 식이다.
- class가 3개인 값의 예를 들어보자. 첫번째 관측치의 label이 [1, 0, 0]라 할 때, 이 벡터는 참 분포인 $ p(x) $ 에 해당한다. 그리고 학습을 통해 얻은 추정치가 [ 0.7, 0.2, 0.1 ]이라 할 때 이 벡터는 후보 분포인 $ q(x) $ 에 해당한다. 따라서 첫 번째 관측치에 대한 크로스엔트로피는 - {1ln(0.7) + 0ln(0.2)+0ln(0.1)}이다.

(2) 관점2. 다항분포의 우도함수 <br/>

- 엔트로피란 마이너스 로그우도함수 ( $ - ln q(x) $ ) 의 기대값이다. 즉, 로그우도함수를 최대화하는 문제는 엔트로피가 최소화하는 문제와 동치이다.
- 다항분포는 k개의 카테고리가 $ p_1,p_2, ..., p_k $ 의 확률로 발생할 수 있고, 카테고리의 사건 발생 횟수가 $ x_1, x_2 ... , x_k $ 일 때의 분포다. 단, 총 독립시행 횟수 $ n $ 은 각 카테고리에서의 발생횟수의 합인 $x_1+x_2+...+x_k $ 다. 딥러닝에서의 손실함수를 여기에 대입해서 생각해보면, k개 카테고리를 가진 다항분포에서 1번 독립시행한 경우다.
 
[다항분포로부터 크로스 엔트로피 도출] <br/>

다항함수의 확률질량함수는 다음과 같다. <br/>
$ p(x_1, x_2,..., x_k|n, p_1, p_2, ..., p_k)=\frac{n!}{x_1!x_2!...x_k!}p_1^{x_1}p_2^{x_2}...p_n^{x_k} $ 
<br/>
로그우도함수는 다음과 같다. <br/>
$ ln p(x_1, x_2,..., x_k|n, p_1, p_2, ..., p_k)=ln\frac{n!}{x_1!x_2!...x_k!} + x_1 ln p_1 + x_2 ln p_2 + ... + x_k ln p_k $ <br/>
독립시행이 1번일 때 $ ln\frac{n!}{x_1!x_2!...x_k!}= ln(1) =0 $ 이다. 0인 항을 소거하고 마이너스 로그를 취하면 우리가 아는 꼴의 크로스 엔트로피가 나온다. <br/>
$ - ln p(x_1, x_2,..., x_k|n, p_1, p_2, ..., p_k)= - (x_1 ln p_1 + x_2 ln p_2 + ... + x_k ln p_k) $ ...(a)

- 3개의 카테고리를 분류하는 문제의 경우를 생각해보자. 어떤 관측치가 1번 클래스이면 label은 [1,0,0]이다. 다항분포로 생각하면, 총 독립시행이 1번일 때, 1번째 카테고리의 사건 발생횟수는 1, 나머지 카테고리의 사건 발생횟수는 0인 경우다. 즉, [ $ x1 $, $ x2 $ , $ x3 $ ]이 [1,0,0]이다. 또한, 각 카테고리의 사건이 발생할 확률  [ $ p1 $, $ p2 $ , $ p3 $ ] 은 우리가 추정한 값의 분포라고 생각하면 된다. 예를 들어 softmax를 거쳐 추정한 값이 [0.7,0.2,0.1]이면 이것이 각 카테고리가 발생할 확률이다. 따라서 이 관측치의 마이너스 로그 우도 함수 값은 (a) 식에 의해 -{1ln(0.7) + 0ln(0.2)+0ln(0.1)} 다. 

<br/>
다음 (3), (4)에서 설명하는 시그모이드와 소프트맥스는 추정값 벡터를 확률 분포처럼 만든다. 시그모이드 혹은 소프트맥스를 거친 벡터는 각 요소가 0과 1사이 값을 가지며 벡터합이 1이다. 확률벡터로 변환 후 크로스엔트로피를 통해 손실함수를 만든다. <br/>

<br/>
참고. 확률의 공리 
> 1. 사건 $ X $ 에 대해 $ P(X) \geq 0 $ 이며 $ X \in S $, $ S $ 는 표본 공간
> 2. 표본 공간 $ S $ 에 대해 $ P(S) = 1 $
> 3. 상호 배반적인 사건 $ X_1, X_2, ..., X_k $ 에 대해 $ P(\bigcup^k_{i=1}X_i)= \sum^k_{i=1}P(X_i) $

<br/>

(3) 시그모이드와 이진분류 <br/>
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Error_Function.svg/1280px-Error_Function.svg.png" width="40%">

- 클래스가 1개(사건이 발생, 즉 해당 label이거나 아니거나로 해석, 다항분포에서의 클래스 수 k=1), 독립시행 1번인 다항분포에 해당한다. 즉 베르누이 분포에 해당한다.
- 시그모이드 함수: $ \sigma(x) = \frac{1}{1+e^{-x}} $
- 클래스에 해당할 확률은 $ \sigma(x) $, 클래스에 해당하지 않을 확률은 $ 1-\sigma(x) $
- 위 시그모이드 변환 그래프를 보면 $ x $ 가 절대값이 큰 음수일 때 변환된 값은 0에 가까워져 graident가 소실된다. 따라서 시그모이드 층을 거친 벡터의 손실함수로 크로스엔트로피를 사용하지 않고 MSE를 쓰면 예측이 매우 부정확해진다.

(4) 소프트맥스와 다항분류
- 클래스가 2개 이상, 독립시행이 1번인 다항분포에 해당한다.
- 소프트맥스 함수: $ k $ 개 요소를 가진 벡터 $ x =(x_1, x_2, ...,x_k) $에 대해 $ \sigma x_i = \frac{ e^{x_i} }{ \sum^k_{j=1} e^{x_j} } $  
		
#### 회귀문제: MSE
- 회귀 문제의 경우 모델을 통해 추정한 값과 참값 간 손실함수로 MSE를 사용한다.
- $ MSE=\frac{1}{n} \sum^n_{i=1} (Y_i - \hat{Y_i})^2 $ , 단 $ Y_i $ 는 참값, $ \hat{Y_i} $ 는 

## 1.3. Hidden Units
이 챕터에서는 은닉층의 활성화함수에 대해 살펴보겟다. <br/>
딥러닝에서 주로 사용하는 활성함수들은 모든 정의역에 대해 미분가능한 것은 아니다. ReLU의 경우 0 이하의 정의역에서는 미분값이 0인데 파라미터를 0으로 추정하는 것은 바라는 바가 아니다. 이 이슈에 대해서는 뒤 챕터 3. Optimization for Training Deep에서 다루겠다.

### 1.3.1. ReLU와 ReLU의 일반화 버전
활성화함수의 역할은 레이어를 통과한 식 $ W^T x+b $ 을 아핀변환 $ g $ 하는 것이다. <br/>
$ h = g(W^T x+b) $   <br/>
단순하게 이야기하자면, 활성화 함수는 레이어를 통과한 결과값의 성질을 유지하되 값의 범위를 shift하는 역할을 한다. 다음 그림을 보면 아핀변환 후 A라는 정보는 유지하되 정보의 각도와 위치가 바뀌었다.<br/>
![Image](https://homepages.inf.ed.ac.uk/rbf/HIPR2/affineb.gif)

<br/>
다음은 기본적인 ReLU와 일반화된 버전의 ReLU다. <br/>

<img src="https://paperswithcode.com/media/methods/new_act.jpg" width="100%">

a. 기본형
- ReLu: $ g(z) = max ( 0,z ) $ 기본 ReLU는 음수의 정의역에 대해 모두 0으로 반환 <br/>

b. 변형 
- $ z_i<0 $ 구간에 대해 활성함수를 가중합 $ h_i = g(z, \alpha)_i = max(0,z_i) + \alpha_i min(0, z_i) $ 으로 수정. 단, $ \alpha $ 는  0이 아닌 기울기   <br/>
- Absolute Value Rectification: $ \alpha=-1 $인 버전. 이 경우 $ g(z) =  \lvert z  \lvert $  <br/>
- Leaky ReLU: $ \alpha $ 값이 0.01과 같이 작은 값으로 고정  <br/>

### 1.3.2. Logistic Sigmoid & Hyperbolic Tangent

<img src="https://www.researchgate.net/profile/Junxi-Feng/publication/335845675/figure/fig3/AS:804124836765699@1568729709680/Commonly-used-activation-functions-a-Sigmoid-b-Tanh-c-ReLU-and-d-LReLU.ppm" width="60%">

- Sigmoid Activation: $ g(z) = \sigma (z) $  <br/> 
binary classification에서 positive일 확률을 구하는 활성화 함수, 절대값이 큰 음수 값은 0에 가까운 값으로 활성화되고, 절대값이 큰 양수 값은 1에 가까운 값으로 활성화(saturation), gradient 기반의 파라미터 추정에서는 파라미터의 전달이 0이면 좋지 않기 때문에, sigmoid 활성화 함수는 주로 hidden layer가 아닌 output layer에 적용됨 <br/> 
- Hyperbolic Tangent Activation: $ g(z) = tanh(z) $   <br/> 
 $ \sigma(0) = \frac{1}{2} $ 인데 반해 $ \tanh(0) = 0 $ <br/> 이라는 점에서 $ z=0 $ 부근에서 tanh가 sigmoid보다 identity 함수(y=x)과 비슷하다. 이러한 특징은 $ tanh $ 활성 함수를 사용했을 때 파라미터 최적화를 하는 학습을 용이하게 한다.
- 두 활성 함수의 관계: $ tanh(z) = 2 \sigma (2z) -1 $

### 1.3.3. Other Hidden Units
<img src="https://www.researchgate.net/profile/Joel-Dapello/publication/325022755/figure/fig9/AS:624102360494083@1525809006987/Alternative-activation-functions.png" width="40%">
<img src="https://atcold.github.io/pytorch-Deep-Learning/images/week11/11-1/Hardtanh.png" width="40%">

- Softmax
- Radial basis function
- Softplus
- Hard tanh

## 1.4. Architecture Design
아키텍쳐란 신경망의 레이어의 개수와 연결 상태 전반의 디자인을 의미한다.
첫번째 층 $ h^{(1)} = g^{(1)} (W^{(1)T}x + b^{(1)} $ , 두번째 층 $ h^{(2)} = g^{(2)} (W^{(2)T} h^{(1)} + b^{(2)} $ , ... 와 같이 대부분의 구조는 chain 구조로 나열된다. 
신경망을 구성할 때 고려할 점은 한 레이어 당 레이어의 width와 레이어 연결의 depth다.

### 1.4.1. Universal Approximation Properties and Depth
선형 모형은 손실함수(선형식의 제곱합 꼴)가 아래로볼록(convex) 함수여서 손실함수를 최소로 만드는 파라미터를 추정하는 것이 쉽다. 반면 신경망은 비선형 함수여서 손실함수를 최소화하는 파라미터를 추정하는 것이 어렵다. **universal approximation theorem**(Hornik et al., 1989; Cybenko, 1989)에 의하면 (1) 선형 output layer와 (2) *squashing 활성화 함수를 가진 (3) 피드포워드 신경망이라면, 어떤 복잡한 함수(Borel 측도 가능 함수라면 어떤 함수라도, (확률)측도론 참조)도 근사할 수 있다. 다시 말하자면, closed & bounded(해석학 참조) N차 실수 공간 상의 연속 함수에 대해 신경망으로 근사할 수 있다. <br/>
<br/>
참고. *squashing 함수: 아웃풋 값을 뭉개는 함수, 시그모이드 함수와 같이 아웃풋을 작은 값의 범위로 변환 <br/>

큰 MLP가 어떤 함수도 근사할 수 있다는 사실과는 별개로 신경망이 **학습**을 통해 제대로 True 함수에 근사할 수 있는지는 보장할 수 없다. 근사하고자 하는 함수에 적절한 최적화 방법을 선택하는 것, 과적합 없이 훈련하는 것 등의 이슈를 해결하지 않는 한 신경망이 True 함수에 근사할 수 없다. 또한 적절한 근사함수를 만들기 위해 얼마나 깊게, 효율적으로 신경망을 만들어야 하는지는 알 수 없다. 




# 2. Regularization for Deep Learning

# 3. Optimization for Training Deep

# 4. Convolutional Networks

# 5. Sequence Modeling: Recurrent and Recursive Nets


# Part2. Deep Learning Research

# 1. Linear Factor Models

# 2. Autoencoders

# 3. Representation Learning

# 4. Structured Probabilistic Models

# 5. Monte Carlo Methods

# 6.Confronting the Partition Function

# 7. Approximate Inference

# 8. Deep Generative Models
