---
layout: single
title: "Diffusion Models"
categories: diffusion-models
mathjax: true
header:
  image: /assets/images/gan.jpeg
  image_description: "Creative"
  teaser: /assets/images/gan.jpeg
---

*Sinh ảnh với mô hình Diffusion*.

---

## Introduction

*Diffusion model* thuộc nhóm mô hình sinh (*generative model*). Hiểu đơn giản thì Diffusion model hoạt động nhờ thay đổi  
training data bằng cách thêm lần lượt Gaussian noise, sau đó đảo ngược noise process này để học cách tái tạo data gốc 
(*Denoising process*). Sau quá trình huấn luyện, Diffusion model có thể dùng để sinh dữ liệu bằng cách sử dụng noise sample 
ngẫu nhiên làm đầu vào của learned denoising process.

Đặc biệt, mô hình Diffusion là mô hình latent variable ánh xạ đến latent space thông qua một *fixed Markov chain*. 
Chuỗi Markov này thêm lần lượt nhiễu vào dữ liệu theo thứ tự xác định để thu được *approximate posterior* 
$q(\textbf{x}_{1:T}|\textbf{x}_0)$, với $\textbf{x}_1, ... , \textbf{x}_T$ là các latent variable có cùng kích thước với ``
$\textbf{x}_0$. 

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-1-1.png){:style="display:block; margin-left:auto; margin-right:auto"}

Ảnh đầu vào được biến đổi về dạng gần như hoàn toàn chỉ là Gaussian noise. Quá trình huấn luyện là nghịch 
đảo quá trình này - tức là huấn luyện $p_\theta(x_{t-1}|x_t)$. Đi ngược chuỗi đó ta sẽ sinh ảnh mới từ noise sample.

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-1-2.png){:style="display:block; margin-left:auto; margin-right:auto"}





<div align="center">.</div> 

---