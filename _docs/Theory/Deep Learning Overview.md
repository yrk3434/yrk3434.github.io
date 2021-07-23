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
- [Deep Learning, Ian Goodfellow et al.](https://github.com/janishar/mit-deep-learning-book-pdf/blob/master/complete-book-pdf/Ian%20Goodfellow%2C%20Yoshua%20Bengio%2C%20Aaron%20Courville%20-%20Deep%20Learning%20(2017%2C%20MIT).pdf)

# 개요

- 딥러닝 프레임워크를 구성하는 요소들의 기본 개념을 이해할 수 있다. 
- 챕터의 순서는 책의 순서를 따르지만, 챕터 내 설명은 재구성하였다.
- 책 내용 밖의 설명은 통계 전공의 지식을 바탕으로 첨언하였다.


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
피드포워드 네트워크란 한 방향(**acyclic**, 신경망의 연결이 순환하지 않음)으로 연결된 네트워크다. Multilayer Perceptrons(MLPs)라고도 불리운다. 피드포워드 네트워크는 일종의 합성함수인데 $ f= f^{(3)}(f^{(2)}(f^{(1)}(x))) $ 와 같은 꼴의 식이다. 각 식 $ f^{(1)} $ , $ f^{(2)} $ , $ f^{(2)} $ 을 신경망의 layer라고 보면 된다.
- 피드포워드 네트워크는 일종의 여러 겹의 합성함수기 때문에 최적값을 구하기 위해 미분을 할 때 chain rule이 사용된다. 

- 퍼셉트론의 한계 <br/>
퍼셉트론은 일종의 **이진 분류기**다. 어떤 **선형식** $ w \cdot x $ 을 기준으로  $ w \cdot x >0 $ 이면  1 아니면 0을 반환한다. 여기서 $ w $ 는 가중치다. 한 층의 퍼셉트론이 갖는 대표적인 한계가 선형으로 분류 되지 않는 XOR 문제다. $ x_1 $ , $ x_2 $ 이 1 또는 0을 갖는 논리값이라고 할 때, XOR은 $ x_1 $ , $ x_2 $ 중 하나만 1일 때 1을 반환하고, 나머지의 경우 0을 반환하는 논리회로다. 아래 그림과 같이 XOR은 하나의 직선으로 두 색깔의 점을 분류할 수 없다. 즉 퍼셉트론으로 XOR 분류 문제를 해결할 수 없다. 

![Image](https://res.cloudinary.com/practicaldev/image/fetch/s--O_kCr-s2--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/lkli02223oqhlac1jetz.png){: height="200"}

결론: 선형식으로 위와 같이 문제를 해결할 수 없을 때, 여러 층의 퍼셉트론(MLP)을 사용하여 **비선형** 식을 만들어 문제를 해결해야 한다.

## 1.2. Gradient-Based Learning

피드포워드 신경망은 비선형 함수이고 그에 따라 **손실함수는 non-convex**다. 그 의미는 손실함수의 최소값을 찾으려면, iterative하게 gradient 최적화를 해야하며 전역 최적값을 보장할 수 없다. <br/>
<br/>
참고. 연속적으로 미분 가능하며 아래로 볼록(convexity)인 함수는 전역 최소값이 항상 존재한다. <br/>
<br/>
참고. 이 블로그 포스팅 [Computational Statistics_Optimization](https://yrk3434.github.io/Theory/Computational%20Statistics_Optimization/) ch2. Optimization of Nonlinear Equations를 읽어보자.

### 1.2.1. 손실 함수
<br/>
#### 분류문제: 크로스 엔트로피

(1) 관점1. 정보이론의 엔트로피 <br/>

참고. 이 블로그 포스팅 [Mathematical Statistics](https://yrk3434.github.io/Theory/Mathematical%20Statistics/)  Shannon Entropy를 읽어보자.

- 학습을 통해 추정한 모델이 실제 함수를 잘 설명하는 모델이라면 추정한 값의 분포가 참 분포에 가까워져 엔트로피가 낮아진다. 
- 실제 분포를 $ p(x) $ , 학습을 통해 얻은 후보 분포를 $ q(x) $ 라 할 때, $ - ln q(x) $ 의 기대값은 $ - \int p(x) ln q(x) dx $ (연속분포의 경우) 또는  $ - \sum p(x) ln q(x) $ (이산분포의 경우)라 할 수 있다. 이것이 크로스 엔트로피다. 이산분포의 경우가 우리가 딥러닝에서 흔히 보는 크로스 엔트로피 식이다.
- class가 3개인 값의 예를 들어보자. 첫번째 관측치의 label이 [1, 0, 0]라 할 때, 이 벡터는 참 분포인 $ p(x) $ 에 해당한다. 그리고 학습을 통해 얻은 추정치가 [ 0.7, 0.2, 0.1 ]이라 할 때 이 벡터는 후보 분포인 $ q(x) $ 에 해당한다. 따라서 첫 번째 관측치에 대한 크로스엔트로피는 - {1xln(0.7) + 0xln(0.2)+0xln(0.1)}이다.

(2) 관점2. 다항분포의 우도함수 <br/>

- 엔트로피란 마이너스 로그우도함수 ( $ - ln q(x) $ ) 의 기대값이다. 즉, 로그우도함수를 최대화하는 문제는 엔트로피가 최소화하는 문제와 동치이다.
- 다항분포는 k개의 카테고리가 $ p_1,p_2, ..., p_k $ 의 확률로 발생할 수 있고, 각 카테고리의 사건 발생 횟수가 $ x_1, x_2 ... , x_k $ 일 때의 분포다. 단, 총 독립시행 횟수 $ n $ 은 각 카테고리에서의 발생횟수의 합인 $x_1+x_2+...+x_k $ 다. 딥러닝에서의 손실함수를 여기에 대입해서 생각해보면, k개 카테고리를 가진 다항분포에서 1번 독립시행한 경우다.
 
[다항분포로부터 크로스 엔트로피 도출] <br/>

다항함수의 확률질량함수는 다음과 같다. <br/>
$ p(x_1, x_2,..., x_k|n, p_1, p_2, ..., p_k)=\frac{n!}{x_1!x_2!...x_k!}p_1^{x_1}p_2^{x_2}...p_n^{x_k} $ 
<br/>
로그우도함수는 다음과 같다. <br/>
$ ln p(x_1, x_2,..., x_k|n, p_1, p_2, ..., p_k)=ln\frac{n!}{x_1!x_2!...x_k!} + x_1 ln p_1 + x_2 ln p_2 + ... + x_k ln p_k $ <br/>
독립시행이 1번일 때 $ ln\frac{n!}{x_1!x_2!...x_k!}= ln(1) =0 $ 이다. 0인 항을 소거하고 마이너스 로그를 취하면 우리가 아는 꼴의 크로스 엔트로피가 나온다. <br/>
$ - ln p(x_1, x_2,..., x_k|n, p_1, p_2, ..., p_k)= - (x_1 ln p_1 + x_2 ln p_2 + ... + x_k ln p_k) $ ...(a)

- 3개의 카테고리를 분류하는 문제의 경우를 생각해보자. 어떤 관측치가 1번 클래스이면 label은 [1,0,0]이다. 다항분포로 생각하면, 총 독립시행이 1번일 때, 1번째 카테고리의 사건 발생횟수는 1, 나머지 카테고리의 사건 발생횟수는 0인 경우다. 즉, [ $ x1 $, $ x2 $ , $ x3 $ ]이 [1,0,0]이다. 또한, 각 카테고리의 사건이 발생할 확률  [ $ p1 $, $ p2 $ , $ p3 $ ] 은 우리가 추정한 값의 분포라고 생각하면 된다. 예를 들어 softmax를 거쳐 추정한 값이 [0.7,0.2,0.1]이면 이것이 각 카테고리가 발생할 확률이다. 따라서 이 관측치의 마이너스 로그 우도 함수 값은 (a) 식에 의해 -{1xln(0.7) + 0xln(0.2)+0xln(0.1)} 다. 
	
(3) 시그모이드와 이진분류

(4) 소프트맥스와 다항분류
		
#### 회귀문제: MSE

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
