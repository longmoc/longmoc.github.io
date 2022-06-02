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

Xét điểm $mathbf{X}$ trong không gian 3D (thấy qua hai ảnh) thu được tại $mathbf{x}$ trên ảnh thứ nhất và $mathbf{x'}$ 
trên ảnh thứ hai. Hai *camera center* lần lượt là $\mathbf{C}$ và $\mathbf{C'}$ tạo thành baseline cho *stereo system*. 
Dễ thấy các điểm $x$, $x'$, $\mathbf{C}$, $\mathbf{C'}$ và $X$ là *đồng phẳng* (*coplanar*) do đó 
$\mathbf{\overrightarrow{Cx}}\cdot \left(\mathbf{\overrightarrow{CC'}}\times\mathbf{\overrightarrow{C'x'}}\right)=0$

...

<div align="center">.</div> 

---