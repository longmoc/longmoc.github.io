---
layout: single
title: "Reinforcement learning I: Markov Decision Process"
categories: reinforcement-learning
mathjax: true
header:
  image: /assets/images/rl.jpeg
  image_description: "Studying"
  teaser: /assets/images/rl.jpeg
---
Mở đầu series Reinforcement Learning (RL) bằng Markov Decision Process(es).

Chúng ta sẽ mô tả lại environment qua các thuộc tính:

* State: trạng thái của environment tại một thời điểm
* Action: Hành động của agent
* Reward: Phần thưởng tương ứng của mỗi state (Action không có reward, mà action sẽ đẩy environment sang state mới và state đó sinh reward).
{: .text-justify}

### Markov Process
Một state *s* được coi là Markov state khi: Tương lai sinh ra từ state đó độc lập có điều kiện (conditionally independent) với quá khứ theo *s* đã biết.
{: .text-justify}

$$ P(s_{t+1}|s_{t}) = P(s_{t+1}|s_1, s_2, ..., s_t) $$

Xác suất chuyển từ trạng thái s sang s' được xác định qua hàm state transition probability function:

$$ \mathrm{P}_{ss'}=P(s_{t+1}=s'|s_{t}=s) $$

$$ \mathrm{P}=\begin{bmatrix}p_{11} & ... & p_{1n} \\ ... & ... & ... \\ p_{n1} & ... & p_{nn} \end{bmatrix} $$

Markov process là một memory-less random process và có thể được mô tả qua tuple (S,P) 
với S là không gian trạng thái *state space* và P là *state transition probability function.*
{: .text-justify}

### Markov Reward Process

Là Markov process với đánh giá thưởng phạt. Có thể định nghĩa Markov Reward Process (MRP) qua tuple (S, P, R, $ \gamma $) 
{: .text-justify}

* S : state space hữu hạn
* P : state transition probability function
* R : reward function $ R_{s} = \mathrm{E}[r_{t+1} \mid s_t=s] $
* $ \gamma $ : discount factor nằm trong khoảng [0, 1].

$$ R_t = \sum_{t'=t+1}^{T}{\gamma^{t'-t-1}r_{t'}} $$

Dễ thấy, $ \gamma=0 $ thể hiện sự short-sign, chỉ quan tâm đến reward kế tiếp. 
Mặt khác $ \gamma=1 $ thể hiện far-sign, quan tâm đến toàn bộ reward tương lai.
{: .text-justify}

Nói thêm về $ \gamma $, giả sử max reward là giá trị *m*, khi đó:

$$ R_t=\sum_{t'=t+1}^{T}{\gamma^{t'-t-1}m}\approx\frac{m}{1-\gamma} $$

Do đó, ta thường dùng $ 0<\gamma<1 $ để đảm bảo hội tụ và tránh vòng lặp vô hạn.

Xuất hiện khái niệm *state-value function* cho MRP:

$$ v_{s} = \mathrm{E}[R_t \mid s_t=s] $$

State-value function thể hiện độ tốt có thể có được khi ở trong trạng thái s. 
Lưu ý về việc sử dụng khái niệm kỳ vọng - expectation - $ \mathrm{E} $, 
lí do là bởi vì có thể sẽ có tính ngẫu nhiên trong quá trình chuyển trạng thái. 
Environment có thể sẽ có một stochastic policy, đồng nghĩa với việc cần tổng hoà các kết quả của 
nhiều action. Hoặc transition function là stochastic, thì việc chuyển trạng thái không phải lúc nào 
cũng đạt xác suất 100%.
{: .text-justify}

### Bellman Equation

Phương trình Bellman - [Bellman equation](https://en.wikipedia.org/wiki/Bellman_equation) được sử dụng để chia nhỏ một bài toán tối ưu động 
thành các bài toán con đơn giản hơn. Bellman equation có dạng:
{: .text-justify}

$$ v(s) = \mathop{max}_{a}(R(s,a) + \gamma v(s')) $$

Sau đây chúng ta sẽ đưa bài toán về dạng Bellman Equation

Đầu tiên, xét immediate reward $ r_{t+1} $ và discounted value của 
$ S_{t+1} $: $ \gamma V(S_{t+1}) $ .

$$ \begin{aligned} v(s) &= \mathrm{E}[R_t \mid s_t=s] \\ &= \mathrm{E}[r_{t+1} + \gamma r_{t+2} + \gamma^{2}r_{t+3} + ... \mid s_t=s] \\ &= \mathrm{E}[r_{t+1} + \gamma R_{t+1} \mid s_t=s] \end{aligned} $$

$$ \begin{aligned} \Rightarrow v(s) &=  \mathrm{E}[r_{t+1}+ \gamma\mathrm{E}[R_{t+1} \mid s_{t+1}=s_{t+1}] \mid s_t=s] \\ \Rightarrow v(s) &= \mathrm{E}[r_{t+1} + \gamma v(s_{t+1}) \mid s_t=s] \end{aligned} $$

Chúng ta đã đưa state-value function về dạng Bellman, có thể đơn giản hoá phương trình và giải theo dynamic programming.
{: .text-justify}

$$ v(s) = \mathrm{R}_s + \gamma \sum_{s' \in S}{\mathrm{P}_{ss'}v(s')} $$

$$ \begin{aligned} v &= \mathrm{R} + \gamma \mathrm{P} \\ v &= (1-\gamma \mathrm{P})^{-1}\mathrm{R} \end{aligned} $$

Độ phức tạp thuật toán là $ O(n^3) $ với $ n $ là số state.

<div align="center">.</div> 

<div align="right"><i>(Còn tiếp)</i></div> 
---