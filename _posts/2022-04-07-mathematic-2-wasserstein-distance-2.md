---
layout: single
title: "Wasserstein distance II: Kantorovich-Rubinstein duality"
categories: 
  - mathematic
tags:
  - earth-mover-distance
  - wasserstein-distance
  - information-theory
  - kantorovich-rubinstein
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
  teaser: /assets/images/mathematic.jpeg
---

*Phần cuối ghi chú cần thiết về khoảng cách Wasserstein: đối ngẫu Kantorovich-Rubinstein*

---

Trong phần trước ta đã nêu ra công thức của Earth-Mover và Wesserstein distance theo kết quả của phương pháp đối ngẫu 
Kantorovich-Rubinstein. Phần này sẽ tìm hiểu công thức đó có được như thế nào.

## Linear programming

Trở lại bài toán tính công $\sum_{x,y}\gamma(x,y) \mathbf{D}(x,y)$, việc tính tổng các phần tử của tích element-wise giữa 
$\gamma$ và $\mathbf{D}$ có thể viết dưới dạng $\vec{\mathbf{D}}^T\vec{\gamma}$, trong đó $\vec{\mathbf{D}}$ và $\vec{\gamma}$ lần lượt là các 
dạng flatten vector của $\mathbf{D}$ và $\gamma$. Khi đó có thể đưa về giải quyết bài toán bằng ***Quy hoạch tuyến tính - Linear 
Programming***. Các *Linear program* là các bài toán được trình bày dưới dạng chính tắc:

> Find a vector $\mathbf{x}$ that maximizes $\mathbf{c}^T\mathbf{x}$ subject to $\mathbf{A}\mathbf{x} \leq \mathbf{b}$ and $\mathbf{x} \geq 0$.

Cụ thể dạng *linear program* trong trường hợp này là tối thiểu hóa $\vec{\mathbf{D}}^T\vec{\gamma}$. Trong đó $\vec{\gamma}$ 
tương đương với $\mathbf{x}$ và $\vec{\mathbf{D}}$ tương đương với $\mathbf{c}$.

Điều kiện ràng buộc $\mathbf{A}\mathbf{x} \leq \mathbf{b}$ tổng hợp từ các điều kiện $\sum_x \gamma(x,y) = P_r(y)$ và 
$\sum_y \gamma(x,y) = P_{\theta}(x)$.

$$\mathbf{b} = \begin{bmatrix} P_r \\ P_\theta\end{bmatrix}$$

Gọi số state của hai phân phối $P_{\theta}$ và $P_r$ là $l$, khi đó $\mathbf{b} \in \mathbb{R}^{2l}$ và 
$\vec{\gamma} \in \mathbb{R}^{l^2}$. $\mathbf{A}\vec{\gamma} = \mathbf{b} \Rightarrow \mathbf{A} \in \mathbb{R}^{2l \times l^2}$.

Ví dụ với $l=3$:

$$
\underbrace{
\left[ \begin{array}{rrr|rrr|rrr}
1 & 1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \cr
0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 & 0 \cr
0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \cr \hline
1 & 0 & 0 & 1 & 0 & 0 & 1 & 0 & 0 \cr
0 & 1 & 0 & 0 & 1 & 0 & 0 & 1 & 0 \cr
0 & 0 & 1 & 0 & 0 & 1 & 0 & 0 & 1 \cr
\end{array}\right]
}_{\mathbf{A}}
\cdot
\underbrace{
\begin{bmatrix}
\gamma(x_1, y_1) \\
\gamma(x_1, y_2) \\
\gamma(x_1, y_3) \\ \hline
\gamma(x_2, y_1) \\
\gamma(x_2, y_2) \\
\gamma(x_2, y_3) \\ \hline
\gamma(x_3, y_1) \\
\gamma(x_3, y_2) \\
\gamma(x_3, y_3) \\
\end{bmatrix}
}_{\vec{\gamma}}
=
\begin{bmatrix}
P_r(y_1) \\
P_r(y_2) \\
P_r(y_3) \\ \hline
P_\theta(x_1) \\
P_\theta(x_2) \\
P_\theta(x_3) \\
\end{bmatrix}
= \underbrace{\begin{bmatrix} P_r \\ P_\theta\end{bmatrix}}_{\mathbf{b}}
$$

Khi đã biết $\mathbf{c}^T$, $\mathbf{A}$ và $\mathbf{b}$ sử dụng *LP* tìm được $\mathbf{x}$ hay $\vec{\gamma}$ tối ưu.

Thông tin thêm về *linear programming* có thể tìm hiểu tại [đây](https://en.wikipedia.org/wiki/Linear_programming).


## Kantorovich-Rubinstein duality

Thực tế cách tối ưu bằng *Linear programming* không khả thi trong trường hợp số lượng các trạng thái là rất nhiều (ví dụ 
số cột đất có thể là $10^6$) hoặc số chiều dữ liệu lớn (hàng ngàn chiều thay vì một chiều). Vì thế tính toán $\gamma$ 
trong trường hợp này là rất khó khăn.

Tuy nhiên nhiều trường hợp việc tìm $\gamma$ là không cần thiết, thay vào đó ta chỉ quan tâm tới một giá trị duy nhất 
là $\mathrm{EMD}$ hay $W$. Ví dụ một ứng dụng của *Wasserstein distance* vào việc huấn luyện mô hình *Generative 
Adversarial Network* (*GAN*): generator network sinh ra phân phối sinh $P_{\theta}$ và giá trị $\mathbb{EMD}$ được sử 
dụng để huấn luyện mạng này. Thông thường, việc huấn luyện này cần tính giá trị $\nabla_{P_\theta} \mathrm{EMD}(P_{\theta}, P_r)$. 
Tuy nhiên $P_\theta$ và $P_r$ không đóng vai trò là các biến của quá trình tối ưu nên tính toán trực tiếp gradient không 
khả khi.

May mắn có một cách tính $\mathrm{EMD}$ tiện dụng hơn. Mỗi *LP* đều có hai dạng thể hiện vấn đề: dạng chính (primal form) 
và dạng đối ngẫu (dual form).

$$\begin{array}{c|c}

\mathbf{primal \ form:} & \mathbf{dual \ form:}\\

\begin{array}{rrcl}
\mathrm{minimize} \ & z & = & \ \mathbf{c}^T \mathbf{x}, \\
\mathrm{so \ that} \ & \mathbf{A} \mathbf{x} & = & \ \mathbf{b} \\
\mathrm{and}\  & \mathbf{x} & \geq &\ \mathbf{0}
\end{array} &

\begin{array}{rrcl}
\mathrm{maximize} \ & \tilde{z} & = & \ \mathbf{b}^T \mathbf{y}, \\
\mathrm{so \ that} \ & \mathbf{A}^T \mathbf{y} & \leq & \ \mathbf{c} \\ \\
\end{array}

\end{array}$$

Mục tiêu $\tilde{z}$ phụ thuộc trực tiếp vào $\mathbf{b}$, đồng nghĩa với việc các phân phối $P_r$ và $P_\theta$ 
giờ đây đã trở thành biến của hàm tối ưu - điều ta không thể đạt được nếu dùng *primal form*. Dễ thấy $\tilde{z}$ là 
cận dưới của $z$:

$$z = \mathbf{c}^T \mathbf{x} \geq \mathbf{y}^T \mathbf{A} \mathbf{x} = \mathbf{y}^T \mathbf{b} = \tilde{z}$$

Đây chính là định lý ***weak duality*** (*đối ngẫu yếu*): 
>*the primal problem has optimal value larger than or equal to 
the dual problem*

Đồng nghĩa với việc *duality gap* là lớn hơn hoặc bằng $0$. Ngoài ra còn có định lý *strong duality* với $z=\tilde{z}$:

>Strong duality is a condition in mathematical optimization in which the primal optimal objective and the dual optimal objective are equal

Trở lại với bài toán Earth-Mover, $\tilde{z}$ cực đại tại $y^*$, đặt:

$$ \mathbf{y^*} = \begin{bmatrix} \mathbf{f} \\ \mathbf{g} \end{bmatrix}$$

với $\mathbf{f}, \mathbf{g} \in \mathbb{R}^d$. Lúc này $\mathrm{EMD}(P_r, P_\theta) = \mathbf{f}^T P_r + \mathbf{g}^T P_\theta$. 
Từ điều kiện ràng buộc $\mathbf{A}^T \mathbf{y} \leq \mathbf{c}$:

$$
\underbrace{
\left[ \begin{array}{rrr|rrr}
1 & 0 & 0 & 1 & 0 & 0 \cr
1 & 0 & 0 & 0 & 1 & 0 \cr
1 & 0 & 0 & 0 & 0 & 1 \cr \hline
0 & 1 & 0 & 1 & 0 & 0 \cr
0 & 1 & 0 & 0 & 1 & 0 \cr
0 & 1 & 0 & 0 & 0 & 1 \cr \hline
0 & 0 & 1 & 1 & 0 & 0 \cr
0 & 0 & 1 & 0 & 1 & 0 \cr
0 & 0 & 1 & 0 & 0 & 1 \cr \hline
\end{array}\right]
}_{\mathbf{A}^T}
\cdot
\underbrace{
\begin{bmatrix}
f(x_1) \\
f(x_2) \\
f(x_3) \\ \hline
g(x_1) \\
g(x_2) \\
g(x_3) \\
\end{bmatrix}
}_{y}
\leq
\underbrace{
\begin{bmatrix}
\mathbf{D}(1,1) \\
\mathbf{D}(1,2) \\
\mathbf{D}(1,3) \\ \hline
\mathbf{D}(2,1) \\
\mathbf{D}(2,2) \\
\mathbf{D}(2,3) \\ \hline
\mathbf{D}(3,1) \\
\mathbf{D}(3,2) \\
\mathbf{D}(3,3) \\
\end{bmatrix}
}_{\mathbf{c}}
$$

$$ \Leftrightarrow f(x_i) + g(x_j) \leq \mathbf{D}_{i,j}$$. 

Biết $\mathbf{D}_{i,i} = 0 \Rightarrow g(x_i) \leq -f(x_i) \Rightarrow f(x_i) + g(x_j) \leq f(x_i) - f(x_j)$. Vì vậy để 
tối ưu đạt cực đại thì $g(x_i) = -f(x_i) \Leftrightarrow g = f$. 

Khi đó ta có thể viết $\mathbf{f}^T P_r + \mathbf{g}^T P_\theta$ thành:

$$\mathbb{E}_{x \sim P_r}[f(x)] - \mathbb{E}_{x \sim P_\theta}[f(x)] \tag{1}\label{1}$$

Mặt khác, lúc này:

$$
\begin{aligned}
&\begin{cases}
f(x_i) - f(x_j) \leq \mathbf{D}_{i,j} \\ 
f(x_i) - f(x_j) \geq -\mathbf{D}_{i,j}
\end{cases} \\
\end{aligned} 
$$

$$ \Leftrightarrow \ |f(x_i) - f(x_j)| \leq \mathbf{D}_{i,j} \tag{2}\label{2} $$

Trước khi tiếp tục hãy đến với khái niệm của ***Lipschitz function***:

> Xét $d_X$ và $d_Y$ là các hàm tính khoảng cách trên không gian $X$ và $Y$. Hàm số $f: X \to Y$ được gọi là $K$-Lipschitz 
> nếu với mọi $x_1, x_2 \in X$,
> 
> $$ d_Y(f(x_1), f(x_2)) \le K d_X(x_1, x_2) $$

Hiểu một cách trực quan thì dộ dốc của hàm *$K$-Lipschitz* không bao giờ vượt quá giá trị $K$. Trở lại với $\eqref{2}$, 
xét $|f(x_i) - f(x_j)|$ là *Euclidean distance* giữa $f(x_i)$ và $f(x_j)$, dễ thấy hàm $f$ là hàm *1-Lipschitz* ($K=1$).
$\mathrm{EMD}(P_{\theta}, P_r)$ giải theo dạng đối ngẫu là cận trên của $\eqref{1}$. Vì thế ta có:

$$ \mathrm{EMD}(P_{\theta}, P_r) = \sup_{\lVert f \lVert_{L \leq 1}} \ \mathbb{E}_{x \sim P_{\theta}}[f(x)] - \mathbb{E}_{x \sim P_r}[f(x)] $$

Tương tự với phân phối liên tục ta cũng có được công thức của $W(P_\theta, P_r)$:

$$ W(P_\theta, P_r) = \sup_{\lVert f \lVert_{L \leq 1}} \ \mathbb{E}_{x \sim P_{\theta}}[f(x)] - \mathbb{E}_{x \sim P_r}[f(x)] $$


Hết
---