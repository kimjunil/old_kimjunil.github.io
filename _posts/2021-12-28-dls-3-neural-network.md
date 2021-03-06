---
layout: post
title: "[밑시딥] 3. 신경망"
date: 2021-12-28 16:27
category: "Deep Learning"
author: Junil
tags: [Machine Learning, Deep Learning]
summary: 밑바닥부터 시작하는 딥러닝 Chapter.3 신경망
use_math: true
---


> 밑바닥부터 시작하는 딥러닝을 보고 공부한 내용을 정리합니다.

## 퍼셉트론
CHAPTER 2에서 공부한 다중 퍼셉트론은 XOR같은 복잡한 함수도 표현할 수 있었다. 그렇지만 weight를 사람이 직접 설정해야 한다는 문제가 있다. ANS, OR게이트 정도의 문제는 진리표를 보면서 인간이 적절한 weight를 입력하는게 가능했지만 더 복잡한 문제는 그런 방법으로 해결하기 어렵다. 신경망은 weight의 적절한 값을 데이터로부터 학습한다. CHAPTER 3에서는 신경망이 입력 데이터가 무엇인지 식별하는 처리과정을 알아본다.

## 퍼셉트론 복습
![](/assets/images/how-to-perform-classification-using-a-neural-network-a-simple-perceptron-example_rk_aac_image1.webp)
위와 같은 구조의 네트워크는 아래의 식으로 나타낼 수 있다.  
$$
y=
\begin{cases}
0(w_1x_1+w_2x_2+b\le0) \\
1(w_1x_1+w_2x_2+b>0)
\end{cases}
$$  
위 식에서 조건 분기의 동작을 함수로 나타내면   
$$
y = h(w_1x_1+w_2x_2+b)
$$  
함수 h를 다시 표현하면 아래와같이 표현할 수 있다.  
$$
h(x) = 
\begin{cases}
0(x\le0) \\
1(x>0)
\end{cases}
$$

## 활성화함수
위에서 본 $h(x)$ 처럼 입력신호의 총합을 출력신호로 변환하는 함수를 활성화함수(activation funcion)이라고 부른다. 이름 그대로 입력신호의 총합이 활성화를 일으키는지 정하는 역할을 한다.  

## 계단함수
앞서 설명된 임계치보다 작으면 0, 크면 1을 반환 
$$
h(x) = 
\begin{cases}
0(x\le0) \\
1(x>0)
\end{cases}
$$
![](/assets/images/image-1.png)
## Sigmoid 함수
자연상수를 이용하여 입력신호를 0~1 사이의 실수값으로 반환
$$
h(x) = {1 \over 1+e^{-x}}
$$
![](/assets/images/image-2.png)
## ReLU
나중에 나오는 Gradient Venising 문제를 해결하기 위해 제안된 활성화함수
$$
h(x) = \max(0, x)
$$
![](/assets/images/image-3.png)

## code
<script src="https://gist.github.com/kimjunil/3b83c6e7ad66543715f38c04fbc5bdf5.js"></script>

## 다차원 배열의 계산과 신경망
신경망은 행렬의 곱으로 계산을 수행할 수 있다.
이때 X와 W의 대응하는 차원의 원소 수가 같아야한다
![](/assets/images/image-4.png)
```python
X = np.array([1,2]) # shape -> (2,)
W = np.array([[1,3,5], [2,4,6]]) # shape -> (2,3)
Y = np.dot(X, W)
```

## 3층 신경망 구현하기
![](/assets/images/image-5.png)
$$
a^{(1)}_1 = w^{(1)}_{11}x_1 + w^{(1)}_{12}x_2 + b^{(1)}_1 \\
a^{(1)}_2 = w^{(1)}_{21}x_1 + w^{(1)}_{22}x_2 + b^{(1)}_2 \\
a^{(1)}_3 = w^{(1)}_{31}x_1 + w^{(1)}_{32}x_2 + b^{(1)}_3 \\
$$
행렬로 표현하면 아래와 같이 표현할 수 있다
$$
A = XW^{(1)}+B^{(1)}
$$
```python
X = np.array([1.0, 2.0])
W1 = np.array([0.1, 0.3, 0.5], [0.2, 0.4, 0.6])
B1 = np.array([0.1, 0.2, 0.3])

A1 = np.dot(X, W1)+B1
```

## code
<script src="https://gist.github.com/kimjunil/229699e592ca2ce152ef1b3bcdbba15c.js"></script>

## 출력층 설계하기
분류/회귀 중 어떤 문제냐에 따라 출력층에서 사용하는 활성화함수가 달라진다.
일반적으로 회귀에는 항등함수, 분류에는 소프트맥스 함수를 사용한다.
항등함수는 입력신호 그대로 출력을 하고 소프트맥스 함수는 아래와 같은 식을 사용한다.
$$
y_k = {\exp(a_k)\over\sum\limits_{i=1}^n\exp(a_i)}
$$

## 소프트맥스 함수 구현
소프트맥스 함수는 지수함수를 사용하기 때문에 식대로 구현하면 오버플로우가 발생한다.
이 문제를 해결하기위해 수식을 개선한다
$$
y_k = {\exp(a_k)\over\sum\limits_{i=1}^n\exp(a_i)}={C\exp(a_k)\over C\sum\limits_{i=1}^n\exp(a_i)} \\
= {\exp(a_k+\log{C})\over\sum\limits_{i=1}^n\exp(a_i+\log{C})}\\
= {\exp(a_k+C')\over\sum\limits_{i=1}^n\exp(a_i+C')}
$$
위 식은 지수함수를 계산할 때 어떤 정수를 더하거나 빼도 결과는 바뀌지 않는다는 것을 보여준다. $C'$에는 어떤 값을 대입해도 되지만 오버플로우를 막을 목적으로는 입력신호 중 최댓값을 이용하는 것이 일반적이다.
```python
def softmax(a):
    c = np.max(a)
    exp_a = mp.exp(a-c)
    sum_exp_a = np.sum(exp_a)
    y = exp_a / sum_exp_a
    
    return y
```
