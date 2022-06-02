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
$\mathbf{x}_i'^{\ \mathbf{T}}\mathbf{F} \mathbf{x}_i = 0$ với 
...

<div align="center">.</div> 

---