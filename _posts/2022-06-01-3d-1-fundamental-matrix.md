---
layout: single
title: "Fundamental Matrix"
categories: 3d-reconstruction
mathjax: true
header:
  image: /assets/images/3d.jpeg
  image_description: "Voxels"
  teaser: /assets/images/3d.jpeg
---

*Fundamental matrix*.

---

## Epipolar Geometry

Xét điểm $\mathbf{X}$ trong không gian 3D (thấy qua hai ảnh) thu được tại $\mathbf{x}$ trên ảnh thứ nhất và $\mathbf{x'}$ 
trên ảnh thứ hai. Hai *camera center* lần lượt là $\mathbf{C}$ và $\mathbf{C'}$ tạo thành baseline cho *stereo system*. 
Dễ thấy các điểm $\mathbf{x}$, $\mathbf{x'}$, $\mathbf{C}$, $\mathbf{C'}$ và $\mathbf{X}$ là *đồng phẳng* (*coplanar*) 
do đó $\mathbf{\overrightarrow{Cx}}\cdot \left(\mathbf{\overrightarrow{CC'}}\times\mathbf{\overrightarrow{C'x'}}\right)=0$. 
Mặt phẳng tạo bởi các điểm đó ký hiệu là $\pi$. Các tia back-project từ $\mathbf{x}$ và $\mathbf{x'}$ giao nhau tại $\mathbf{X}$.

![Coplanar]({{ site.url }}{{ site.baseurl }}/assets/images/posts/3d-1-1.png){:style="display:block; margin-left:auto; margin-right:auto"}


Giả sử chúng ta chỉ biết trước điểm $\mathbf{x}$ mà không biết $\mathbf{x'}$. $\mathbf{x'}$ nằm trên mặt phẳng $\pi$ 
được định nghĩa qua camera baseline $\mathbf{CC'}$ và $\mathbf{\overrightarrow{Cx}}$ $\Rightarrow$ $\mathbf{x'}$ nằm 
trên đường thẳng $\mathbf{l'}$ là giao tuyến của $\pi$ và mặt phẳng ảnh thứ hai. Có thể thấy trên ảnh thứ hai 
$\mathbf{l'}$ chính là hình ảnh của tia back-projected từ $\mathbf{x}$. $\mathbf{l'}$ được gọi là *epipolar line* tương ứng 
với $\mathbf{x}$. Điều này có ý nghĩa trong việc giới hạn tìm kiếm các điểm $\mathbf{x'}$ trên $\mathbf{l'}$.

* **Epipole** là giao điểm của baseline $\mathbf{CC'}$ với các mặt phẳng ảnh (Ví dụ $\mathbf{e}$ và $\mathbf{e'}$).
* **Epipolar plane** chỉ các mặt phẳng chứa baseline.
* **Epipolar line** là giao tuyến giữa *epipolar plane* và mặt phẳng ảnh. *Tất cả các epipolar line trên cùng mặt phẳng 
  ảnh giao nhau tại epipole*.
  
## Fundamental Matrix

*Fundamental Matrix* $\mathbf{F}$ là biểu diễn đại số của *epipolar geometry*, mang cả tính hình học và số học.
$\mathbf{x}_i'^{\ \mathbf{T}}\mathbf{F} \mathbf{x}_i = 0$ với $i=1,2,....,m$. Công thức này gọi là *epipolar constraint* 
hoặc *correspondance condition* (hay *Longuet-Higgins* equation). $\mathbf{F}$ là ma trận $3 \times 3$ nên ta có thể 
viết dưới dạng hệ phương trình tuyến tính thuần nhất:

$$ \begin{bmatrix} x'_i & y'_i & 1 \end{bmatrix}
\begin{bmatrix}f_{11} & f_{12} & f_{13} \\ f_{21} & f_{22} & f_{23} \\ f_{31} & f_{32} & f_{33} \end{bmatrix}
\begin{bmatrix} x_i \\ y_i \\ 1 \end{bmatrix} = 0 $$

hoặc:

$$
\begin{bmatrix} x_1 x'_1 & x_1 y'_1 & x_1 & y_1 x'_1 & y_1 y'_1 & y_1 &  x'_1 & y'_1 & 1 \\ \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots \\ x_m x'_m & x_m y'_m & x_m & y_m x'_m & y_m y'_m & y_m &  x'_m & y'_m & 1 \end{bmatrix}\begin{bmatrix} f_{11} \\ f_{21} \\ f_{31} \\ f_{12} \\ f_{22} \\ f_{32} \\ f_{13} \\f_{23} \\ f_{33}\end{bmatrix} = 0
$$

Ký hiệu $$ \mathbf{a^T} = \begin{bmatrix} x_i x'_i & x_i y'_i & x_i & y_i x'_i & y_i y'_i & y_i &  x'_i & y'_i & 1 \end{bmatrix}
\Rightarrow \mathbf{a^T} \cdot \mathbf{f} = 0$$.

Mỗi cặp điểm ảnh *correspondences* $\mathbf{x}$, $\mathbf{x'}$ sẽ tạo ra một vector hệ số $\mathbf{a}$ ứng với một 
constraint. Hệ thuần nhất này cần ít nhất 8 điểm để tìm nghiệm, do đó nó còn được gọi là [Eight-point algorithm.](https://en.wikipedia.org/wiki/Eight-point_algorithm).
Nhóm $N$ *correspondences point* (với $N \geq 8$) - tương ứng $N$ vector $\mathbf{a}$ theo hàng thành ma trận hạng số 
$\mathbf{A}$ ta được $$\mathbf{A} \cdot \mathbf{f} = 0$$.

Lúc này việc giải hệ thuần nhất có thể thực hiện thông qua giải bài toán *linear least squares* sử dụng ***Singular Value 
Decomposition*** (*SVD*) (tìm hiểu thêm về [SVD](https://machinelearningcoban.com/2017/06/07/svd/))

Khi áp dụng SVD cho ma trận $\mathbf{A}$ thu được $\mathbf{USV^T}$ với $\mathbf{U}$ và $\mathbf{V}$ là các ma trận trực 
giao và ma trận đường chéo $\mathbf{S}$ chứa các *singular value*. Các giá trị riêng $\sigma_i$ ($i\in[1,9], i\in\mathbb{Z}$) 
là các giá trị dương và giảm dần, $\sigma_9=0$ vì chỉ có 8 phương trình cho 9 ẩn.
...

<div align="center">.</div> 

---