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
$\textbf{x}_0$. Quá trình này là *forward process* hoặc *diffusion process*.

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-1-1.png){:style="display:block; margin-left:auto; margin-right:auto"}

Ảnh đầu vào được biến đổi về dạng gần như hoàn toàn chỉ là Gaussian noise. Quá trình huấn luyện là nghịch 
đảo của *forward process* - tức là huấn luyện $p_\theta(x_{t-1}|x_t)$. Đi ngược chuỗi đó ta sẽ sinh ảnh mới từ noise sample.

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-1-2.png){:style="display:block; margin-left:auto; margin-right:auto"}

Các mô hình Diffusion đã đạt State-of-the-Art trong nhóm mô hình sinh ảnh. Việc không yêu cầu adversarial training 
giúp mô hình này có lợi thế về scalability và parallelizability.


## Mathematical theory

Như đã giới thiệu, Diffusion model bao gồm *forward process* (*diffusion process*), lần lượt làm nhiễu dữ liệu (thường 
là ảnh), và *reverse process* (*reverse diffusion process*), biến đổi nhiễu thành dạng sample thuộc phân phối đích.

Quá trình chuyển đổi chuỗi lấy mẫu của forward process có thể được đặt thành conditional Gaussian khi noise level đủ 
thấp. Kết hợp điều này với giả thuyết Markov dẫn đến một tham số hóa đơn giản của forward process:

$$q(\textbf{x}_{1:T}|\textbf{x}_0):=\prod_{t=1}^{T}q(\textbf{x}_t|\textbf{x}_{t-1})=\prod_{t=1}^{T}\mathcal{N}(\textbf{x}_t\middle|\sqrt{1-\beta_t}\textbf{x}_{t-1},\beta_t\mathbf{I})$$

Nhắc lại về phân phối Gaussian đơn biến:

$$\mathcal{N}(x|) = \frac{1}{(2\pi \sigma^2)^{1/2}}exp\left\-\frac{1}{2\sigma^2}(x-){\right\}$$

<div align="center">.</div> 

---