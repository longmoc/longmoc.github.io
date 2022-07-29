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
{: .text-justify}

---

## Introduction

*Diffusion model* thuộc nhóm mô hình sinh (*generative model*). Hiểu đơn giản thì Diffusion model hoạt động nhờ thay đổi 
training data bằng cách thêm lần lượt Gaussian noise, sau đó đảo ngược noise process này để học cách tái tạo data gốc 
(*Denoising process*). Sau quá trình huấn luyện, Diffusion model có thể dùng để sinh dữ liệu bằng cách sử dụng noise sample 
ngẫu nhiên làm đầu vào của learned denoising process.
{: .text-justify}

Đặc biệt, mô hình Diffusion là mô hình latent variable ánh xạ đến latent space thông qua một *fixed Markov chain*. 
Chuỗi Markov này thêm lần lượt nhiễu vào dữ liệu theo thứ tự xác định để thu được *approximate posterior* 
$q(\textbf{x}_{1:T}|\textbf{x}_0)$, với $\textbf{x}_1, ... , \textbf{x}_T$ là các latent variable có cùng kích thước với ``
$\textbf{x}_0$. Quá trình này là *forward process* hoặc *diffusion process*.
{: .text-justify}

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-1-1.png){:style="display:block; margin-left:auto; margin-right:auto"}

Ảnh đầu vào được biến đổi về dạng gần như hoàn toàn chỉ là Gaussian noise. Quá trình huấn luyện là nghịch 
đảo của *forward process* - tức là huấn luyện $p_\theta(x_{t-1}|x_t)$. Đi ngược chuỗi đó ta sẽ sinh ảnh mới từ noise sample.
{: .text-justify}

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-1-2.png){:style="display:block; margin-left:auto; margin-right:auto"}

Các mô hình Diffusion đã đạt State-of-the-Art trong nhóm mô hình sinh ảnh. Việc không yêu cầu adversarial training 
giúp mô hình này có lợi thế về scalability và parallelizability.
{: .text-justify}


## Mathematical theory

Như đã giới thiệu, Diffusion model bao gồm *forward process* (*diffusion process*), lần lượt làm nhiễu dữ liệu (thường 
là ảnh), và *reverse process* (*reverse diffusion process*), biến đổi nhiễu thành dạng sample thuộc phân phối đích.
{: .text-justify}

Quá trình chuyển đổi chuỗi lấy mẫu của forward process có thể được đặt thành conditional Gaussian khi noise level đủ 
thấp. Kết hợp điều này với giả thuyết Markov dẫn đến một tham số hóa đơn giản của forward process:
{: .text-justify}

$$q(\textbf{x}_{1:T}|\textbf{x}_0):=\prod_{t=1}^{T}q(\textbf{x}_t|\textbf{x}_{t-1})=\prod_{t=1}^{T}\mathcal{N}(\textbf{x}_t|\sqrt{1-\beta_t}\textbf{x}_{t-1},\beta_t\mathbf{I})$$

Với $\beta_1, ..., \beta_T$ là *variance schedule* (cố định hoặc được huấn luyện) mục đích đảm bảo $x_T$ tiến tới 
một Gaussian đẳng hướng với $T$ đủ lớn.
{: .text-justify}

>Nhắc lại về phân phối Gaussian đơn biến:
>
>$$\mathcal{N}(x|\mu,\sigma) = \frac{1}{(2\pi \sigma^2)^{1/2}}\ \exp\bigg(-\frac{1}{2\sigma^2}(x-\mu)^2\bigg)$$
>
>Áp dụng công thức [LTP](https://longmoc.github.io/mathematic/mathematic-4-conv-probability-distribution/)
>
>$$
\begin{aligned} 
p_{X_t}(x_t) &= \int p_{X_t|X_{t-1}}(x_t|x_{t-1})p_{X_{t-1}}(x_{t-1}) \ dx_{t-1} \\
&= \int \mathcal{N}(x_t|x_{t-1}, 1)p_{X_{t-1}}(x_{t-1}) \ dx_{t-1} \\
&= \int \mathcal{N}(x_t - x_{t-1}|0, 1)p_{X_{t-1}}(x_{t-1}) \ dx_{t-1}
\end{aligned}
$$
>
>Có thể thấy biểu thức cuối là định nghĩa toán học của [convolution](https://longmoc.github.io/mathematic/mathematic-4-conv-probability-distribution/#convolution), 
do đó:
>
>$$p_{X_t}(x_t) = (\mathcal{N}(0,1) * p_{X_{t-1}})(x_t)$$
>
>dẫn đến:
>
>$$X_t = \mathcal{N}(0,1) + X_{t-1}$$
>
>Vì thế tại mỗi bước, ảnh bị làm nhiễu bằng cộng thêm Gaussian noise:
>
>$$\textbf{x}_t \sim \mathcal{N}(x|0,1) \iff \textbf{x}_t = \textbf{x}_{t-1} + \mathcal{N}(0,1)$$
> 
> (Ta tạm bỏ qua variance scheduled để đơn giản việc chứng minh)

Trong quá trình huấn luyện, mô hình sẽ học cách đảo ngược quá trình diffusion trên để tạo ra ảnh mới.
Bắt đầu với Gaussian noise thuần túy $$p(\textbf{x}_{T}) = \mathcal{N}(\textbf{x}_T, \textbf{0}, \textbf{I})$$, mô hình 
sẽ học được phân phối $p_\theta(\textbf{x}_{0:T})$ theo:
{: .text-justify}

$$
\begin{aligned}
p_\theta(\textbf{x}_{0:T}) &= p(\textbf{x}_T)\prod_{t=1}^{T}p_\theta(\textbf{x}_{t-1}|\textbf{x}_t) \\
&= p(\textbf{x}_T)\prod_{t=1}^{T}\mathcal{N}\big(\textbf{x}_{t-1}|\mu_\theta(\textbf{x}_t, t), \Sigma_\theta(\textbf{x}_t, t)\big)
\end{aligned}
$$

Những tham số phụ thuộc theo thời gian của Gaussian transition sẽ được mô hình huấn luyện và phân phối reverse diffusion 
transition chỉ phụ thuộc vào timestep trước vì tính chất của Markov.
{: .text-justify}

## Optimizing

Mô hình Diffusion được huấn luyện nhằm tìm ra reverse Markov transition tối đa hóa likelihood của training data. Trong 
thực tế điều này tương đương với việc tối thiểu hóa giới hạn trên của âm ($-$) log likelihood.
{: .text-justify}

$$\mathbb{E}\big[-\log p_\theta(\textbf{x}_0)\big] \leq \mathbb{E}_q\bigg[-\log \frac{p_\theta(\textbf{x}_{0:T})}{q(\textbf{x}_{1:T}|\textbf{x}_0)}\bigg] = L_{vlb}$$

*(Dễ thấy $L_{vlb}$ chính là giới hạn trên cần tối thiểu, đây cũng chính là âm của [Evidence lower bound - ELBO](https://en.wikipedia.org/wiki/Evidence_lower_bound))*
{: .text-justify}

Từ khai triển $p_\theta(\textbf{x}_{0:T})$ và Markov ở trên ta có:

$$
\begin{aligned}
\mathbb{E}_q\bigg[-\log \frac{p_\theta(\textbf{x}_{0:T})}{q(\textbf{x}_{1:T}|\textbf{x}_0)}\bigg]  &= 
\mathbb{E}_q\bigg[-\log p(\textbf{x}_T) - \log \frac{\prod_{t=1}^{T}p_\theta(\textbf{x}_{t-1}|\textbf{x}_t)}{\prod_{t=1}^{T}q(\textbf{x}_t|\textbf{x}_{t-1})}\bigg] \\
&= \mathbb{E}_q\bigg[-\log p(\textbf{x}_T) - \log \frac{p_\theta(\textbf{x}_0|\textbf{x}_1)}{q(\textbf{x}_1|\textbf{x}_0)} - 
\sum_{t=2}^{T} \log \frac{p_\theta(\textbf{x}_{t-1}|\textbf{x}_t)}{q(\textbf{x}_t|\textbf{x}_{t-1})}\bigg]
\end{aligned}
\tag{1}\label{1} 
$$

<div align="center">.</div> 

---