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

# Linear programming

Trở lại bài toán tính công $$\sum_{x,y}\gamma(x,y) D(x,y)$$, việc tính tổng các phần tử của tích element-wise giữa 
$\gamma$ và $D$ có thể viết dưới dạng $\vec{D}^T\vec{\gamma}$, trong đó $\vec{D}$ và $\vec{\gamma}$ lần lượt là các 
dạng flatten vector của $D$ và $\gamma$. Khi đó có thể đưa về giải quyết bài toán bằng ***Quy hoạch tuyến tính - Linear 
Programming***. Các *Linear program* là các bài toán được trình bày dưới dạng chính tắc:

> Find a vector $\mathbf{x}$ that maximizes $\mathbf{c}^T\mathbf{x}$ subject to $\mathbf{A}\mathbf{x} \leq \mathbf{b}$ and $\mathbf{x} \geq 0$.

Cụ thể dạng *linear program* trong trường hợp này là tối thiểu hóa $\vec{D}^T\vec{\gamma}$. Trong đó $\vec{\gamma}$ 
tương đương với $\mathbf{x}$ và $\vec{D}$ tương đương với $\mathbf{c}$.

Điều kiện ràng buộc $\mathbf{A}\mathbf{x} \leq \mathbf{b}$ tổng hợp từ các điều kiện $\sum_x \gamma(x,y) = P_{\theta}(y)$ và 
$\sum_y \gamma(x,y) = P_r(x)$.

$$\mathbf{b} = \begin{bmatrix} P_r \\ P_\theta\end{bmatrix}$$

Gọi số state của hai phân phối $P_{\theta}$ và $P_r$ là $l$, khi đó kích thước của $\mathbf{b}$ là $2l$, của $\gamma$ là 
$1 \times l^2$.

$$\mathbf{A}\vec{\gamma} = \mathbf{b} $$

$\Rightarrow$ kích thước của $\mathbf{A}$ là $$

Thông tin thêm về *linear programming* có thể tìm hiểu tại [đây](https://en.wikipedia.org/wiki/Linear_programming).


# Kantorovich-Rubinstein duality

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

Mục tiêu $\tilde{z}$ phụ thuộc trực tiếp vào $b\mathbf{b}$, đồng nghĩa với việc các phân phối $P_r$ và $P_\theta$ 
giờ đây đã trở thành biến của hàm tối ưu - điều ta không thể đạt được nếu dùng *primal form*. Dễ thấy $\tilde{z}$ là 
cận dưới của $z$:

$$z = \mathbf{c}^T \mathbf{x} \geq \mathbf{y}^T \mathbf{A} \mathbf{x} = \mathbf{y}^T \mathbf{b} = \tilde{z}$$

Đây chính là định lý ***weak duality*** (*đối ngẫu yếu*): 
>*the primal problem has optimal value larger than or equal to 
the dual problem*

Đồng nghĩa với việc *duality gap* là lớn hơn hoặc bằng $0$. Ngoài ra còn có định lý *strong duality* với $z=\tilde{z}$:

>Strong duality is a condition in mathematical optimization in which the primal optimal objective and the dual optimal objective are equal



---

<div align="right"><i>_Hết_</i></div> 

---