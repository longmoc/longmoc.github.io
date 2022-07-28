---
layout: single
title: "Convolution of Probability Distribution"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
  teaser: /assets/images/mathematic.jpeg
---

*Cách tìm ra phân phối tổng của hai biến ngẫu nhiên độc lập sử dụng Convolution*

---

Trước tiên hãy nhắc lại Law of Total Probability (LTP) cho biến ngẫu nhiên rời rạc:

Với $X$, $Y$ là hai biến ngẫu nhiên rời rạc:

$$ p_X(x) = \sum_y p_{X,Y}(x,y) = \sum _y p_{X|Y}(x|y)p_Y(y)$$

Đối với biến liên tục, nếu $X$, $Y$ là hai biến ngẫu nhiên liên tục:

$$ f_X(x) = \int_{-\infty}^{+\infty} f_{X,Y}(x,y) \ dy = \int_{-\infty}^{+\infty} f_{X|Y}(x|y)f_Y(y) \ dy$$

## Convolution

Trong xác suất thống kê, convolution là phép toán cho phép ta lấy được phân phối tổng của hai biến rời rạc. Giả sử 
một công ty khai thác có sản lượng vàng khai thác được là $X$ tấn mỗi năm ở nước A và $Y$ tấn mỗi năm ở nước B. việc 
khai thác của công ty giữa các nước này là hoàn toàn độc lập với nhau, và bạn đã có một vài phân phối để mô hình hóa chúng. 
Vậy phân phối tổng lượng vàng khai thác được, $Z = X+Y$, là gì?

Để giải những bài toán như trên, hay tìm hiểu một ví dụ cụ thể như sau:

Cho $X,Y \sim Unif(1,6)$ là các lần tung độc lập của xí ngầu 6 mặt. Tính hàm khối xác suất (PMF) của $Z=X+Y$

Phạm vi các trường hợp của Z là tổng của hai xí ngầu trong đó kết quả mỗi xí ngầu thuộc ${1, 2, 3, 4, 5, 6}$:

$$\Omega_Z = {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12} $$

Xác suất các trường hợp này là không đồng nhất, chỉ có một cách đổ hai xí ngầu ra tổng bằng 2 nhưng có 6 cách để đổ ra 
tổng bằng 7. Muốn tính xác suất mỗi trường hợp ta phải tính tổng tất cả các khả năng có thể của $X$ trong $\Omega_X{1,2,3,4,5,6}$

$$\begin{aligned} 
\mathbb{P}(Z=3) &= \mathbb{P}(X=1,Y=2) + \mathbb{P}(X=2,Y=1) + \mathbb{P}(X=3,Y=0) + \mathbb{P}(X=4,Y=-1) + \mathbb{P}(X=5,Y=-2) + \mathbb{P}(X=6,Y=-3) \\
&= \mathbb{P}(X=1)\mathbb{P}(Y=2) + \mathbb{P}(X=2)\mathbb{P}(Y=1) + \mathbb{P}(X=3)\mathbb{P}(Y=0) + 
\mathbb{P}(X=4)\mathbb{P}(Y=-1) + \mathbb{P}(X=5)\mathbb{P}(Y=-2) + \mathbb{P}(X=6)\mathbb{P}(Y=-3) \\
&= \frac{1}{6}\cdot\frac{1}{6} + \frac{1}{6}\cdot\frac{1}{6} + \frac{1}{6}\cdot 0 + \frac{1}{6}\cdot 0 + \frac{1}{6}\cdot 0 + \frac{1}{6}\cdot 0 \\
&= \frac{1}{18}
\end{aligned}$$

Dạng tổng quát hơn, để tính $p_{Z}(z)=\mathbb{P}(Z=z)$ với mọi $z$ ta dùng:

$$\begin{aligned} 
p_{Z}(z) &= \mathbb{P}(Z=2) \\
&= \sum_{x\in\Omega_X}\mathbb{P}(X=x,Y=z-x) \\
&= \sum_{x\in\Omega_X}\mathbb{P}(X=x)\mathbb{P}(Y=z-x) \\
&= \sum_{x\in\Omega_X}p_X(x)p_Y(z-x)
\end{aligned}$$


<div align="center">.</div>
---