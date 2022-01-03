---
layout: post
title: "[밑시딥] 2. 퍼셉트론"
date: 2021-12-22 06:27
category: "Deep Learning"
author: Junil
tags: [Machine Learning, Deep Learning]
summary: 밑바닥부터 시작하는 딥러닝 Chapter.2 퍼셉트론
use_math: true
---


> 밑바닥부터 시작하는 딥러닝을 보고 공부한 내용을 정리합니다.

## 퍼셉트론
신경명의 기원이 되는 알고리즘.
다수의 (흐름이 있는)신호를 입력받아 하나의 신호를 출력한다. 
![](/assets/images/how-to-perform-classification-using-a-neural-network-a-simple-perceptron-example_rk_aac_image1.webp)
*[출처](https://www.allaboutcircuits.com/technical-articles/how-to-perform-classification-using-a-neural-network-a-simple-perceptron-example/)*

노드의 입력과 weight를 곱한 것을 모두 합한 값이 임계치(θ)를 넘기면 1을 출력하고 그렇지 않으면 0을 출력한다.  
이를 수식으로 표현하면 다음과 같다.  
$$
y = 
\begin{cases}
0(w_1x_1+w_2x_2\le\theta) \\
1(w_1x_1+w_2x_2>\theta)
\end{cases}
$$

퍼셉트론은 입력신호 각각에 고유한 weight를 부여하고 이것은 각 신호가 결과에 주는 영향력을 조절하는 요소로 작용한다.

위 식에서 θ를 -b(bias)로 치환하면 아래와 같은 식이 된다.  
$$
y=
\begin{cases}
0(w_1x_1+w_2x_2+b\le0) \\
1(w_1x_1+w_2x_2+b>0)
\end{cases}
$$  
다시 말하면 weight는 각 입력신호가 결과에 주는 영향력, bias는 뉴런이 얼마나 쉽게 활성화하는지를 결정한다.

# 퍼셉트론의 한계
퍼셉트론은 직선으로 분류할 수 있는 AND, NAND, OR 논리회로를 표현할 수 있지만 비선형적인 XOR 게이트 같은 것들은 표현할 수 없다.
![](/assets/images/1_Tc8UgR_fjI_h0p3y4H9MwA.png)
*[출처](https://yceffort.kr/2019/02/19/pytorch-03-perceptron)*

# 다층 퍼셉트론
아래 그림과 같이 퍼셉트론을 여러층 쌓는다면 단층 퍼셉트론으로는 표현할 수 없는 비선형 영역도 표현할 수 있다.
![](/assets/images/xor_solution.png)
![](/assets/images/xor_2layers.png)
이처럼 퍼셉트론은 층을 깊게하여 더 다양한 것을 표현할 수 있다.