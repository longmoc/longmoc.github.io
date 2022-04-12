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
dạng flatten vector của $D$ và $\gamma$. Khi đó có thể đưa về giải quyết bài toán bằng ***Linear Programming***. Các 
*Linear program* là các bài toán được trình bày dưới dạng chính tắc:

> Find a vector $x$ that maximizes $c^Tx$ subject to $Ax \leq b$ and $x \geq 0$.

Cụ thể dạng *linear program* trong trường hợp này là tối thiểu hóa $\vec{D}^T\vec{\gamma}$. Trong đó $\vec{\gamma}$ 
tương đương với $x$ và $\vec{D}$ tương đương với $c$.

Điều kiện ràng buộc $Ax \leq b$ tổng hợp từ các điều kiện $\sum_x \gamma(x,y) = P_{\theta}(y)$ và 
$\sum_y \gamma(x,y) = P_r(x)$. 

Thông tin thêm về *linear programming* có thể tìm hiểu tại [đây](https://en.wikipedia.org/wiki/Linear_programming).


# Kantorovich-Rubinstein duality

...

---

<div align="right"><i>_Hết_</i></div> 

---