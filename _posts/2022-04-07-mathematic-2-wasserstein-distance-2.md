---
layout: posts
title: "Wasserstein distance II: Kantorovich-Rubinstein duality"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
---

*Phần cuối ghi chú cần thiết về khoảng cách Wasserstein: đối ngẫu Kantorovich-Rubinstein*

---

# Linear programming

Trở lại bài toán tính công $$\sum_{x,y}\gamma(x,y) D(x,y)$$, việc tính tổng các phần tử của tích element-wise giữa 
$\gamma$ và $D$ có thể viết dưới dạng $\vec{D}^T\vec{\gamma}$, trong đó $\vec{D}$ và $\vec{\gamma}$ lần lượt là các 
dạng flatten vector của $D$ và $\gamma$. Khi đó có thể đưa về giải quyết bài toán bằng ***Quy hoạch tuyến tính - Linear 
Programming***. Các *Linear program* là các bài toán được trình bày dưới dạng chính tắc:

> Find a vector $x$ that maximizes $c^Tx$ subject to $Ax \leq b$ and $x \geq 0$.

Cụ thể dạng *linear program* trong trường hợp này là tối thiểu hóa $\vec{D}^T\vec{\gamma}$. Trong đó $\vec{\gamma}$ 
tương đương với $x$ và $\vec{D}$ tương đương với $c$.

Điều kiện ràng buộc $Ax \leq b$ tổng hợp từ các điều kiện $\sum_x \gamma(x,y) = P_{\theta}(y)$ và 
$\sum_y \gamma(x,y) = P_r(x)$.

Thông tin thêm về *linear programming* có thể tìm hiểu tại [đây](https://en.wikipedia.org/wiki/Linear_programming).


# Kantorovich-Rubinstein duality

Thực tế cách tối ưu bằng *Linear programming* không khả thi trong trường hợp số lượng các trạng thái là rất nhiều (ví dụ 
số cột đất có thể là $10^6$) hoặc số chiều dữ liệu lớn (hàng ngàn chiều thay vì một chiều). Vì thế tính toán $\gamma$ 
trong trường hợp này là rất khó khăn.

Tuy nhiên nhiều trường hợp việc tìm $\gamma$ là không cần thiết, thay vào đó ta chỉ quan tâm tới một giá trị duy nhất 
là $\mathbb{EMD}$ hay $W$. Ví dụ một ứng dụng của *Wasserstein distance* vào việc huấn luyện mô hình *Generative 
Adversarial Network* (*GAN*): generator network sinh ra phân phối sinh $P_{\theta}$ và giá trị $\mathbb{EMD}$ được sử 
dụng để huấn luyện mạng này. Thông thường, việc huấn luyện này cần tính giá trị $\nabla_{P_\theta} \mathrm{EMD}(P_{\theta}, P_r)$. 
Tuy nhiên $P_\theta$ và $P_r$ không đóng vai trò là các biến của quá trình tối ưu nên tính toán trực tiếp gradient không 
khả khi.

May mắn có một cách tính $\mathbb{EMD}$ tiện dụng hơn. Mỗi *LP* đều có hai dạng thể hiện vấn đề: dạng chính (primal form) 
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



---

<div align="right"><i>_Hết_</i></div> 

---