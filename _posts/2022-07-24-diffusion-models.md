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

Ý tưởng về mô hình Diffusion có lẽ bắt đầu từ bài báo của Sohl-Dickstein, 
[Deep Unsupervised Learning using Nonequilibrium Thermodynamics](https://arxiv.org/abs/1503.03585), 2015 
nhưng thực sự phổ biến và trở nên hoàn thiện từ bài báo của J. Ho [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) năm 2020.
{: .text-justify}


## Background
#### Diffusion theory

Như đã giới thiệu, Diffusion model bao gồm *forward process* (*diffusion process*), lần lượt làm nhiễu dữ liệu (thường 
là ảnh), và *reverse process* (*reverse diffusion process*), biến đổi nhiễu thành dạng sample thuộc phân phối đích.
{: .text-justify}

Quá trình chuyển đổi chuỗi lấy mẫu của forward process có thể được đặt thành conditional Gaussian khi noise level đủ 
thấp. Kết hợp điều này với giả thuyết Markov dẫn đến một tham số hóa đơn giản của forward process:
{: .text-justify}

$$q(\textbf{x}_{1:T}|\textbf{x}_0)=\prod_{t=1}^{T}q(\textbf{x}_t|\textbf{x}_{t-1})=\prod_{t=1}^{T}\mathcal{N}(\textbf{x}_t;\sqrt{1-\beta_t}\textbf{x}_{t-1},\beta_t\mathbf{I})$$

Với $\beta_1, ..., \beta_T$ là *variance schedule* (cố định hoặc được huấn luyện) mục đích đảm bảo $x_T$ tiến tới 
một Gaussian đẳng hướng với $T$ đủ lớn.
{: .text-justify}

>Nhắc lại về phân phối Gaussian đơn biến:
>
>$$\mathcal{N}(x;\mu,\sigma) = \frac{1}{(2\pi \sigma^2)^{1/2}}\ \exp\bigg(-\frac{1}{2\sigma^2}(x-\mu)^2\bigg)$$
>
>Áp dụng công thức [LTP](https://longmoc.github.io/mathematic/mathematic-4-conv-probability-distribution/)
>
>$$
\begin{aligned} 
p_{X_t}(x_t) &= \int p_{X_t|X_{t-1}}(x_t|x_{t-1})p_{X_{t-1}}(x_{t-1}) \ dx_{t-1} \\
&= \int \mathcal{N}(x_t;x_{t-1}, 1)p_{X_{t-1}}(x_{t-1}) \ dx_{t-1} \\
&= \int \mathcal{N}(x_t - x_{t-1};0, 1)p_{X_{t-1}}(x_{t-1}) \ dx_{t-1}
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
>$$\textbf{x}_t \sim \mathcal{N}(x_{t-1};0,1) \iff \textbf{x}_t = \textbf{x}_{t-1} + \mathcal{N}(0,1)$$
> 
> (Ta tạm bỏ qua variance scheduled để đơn giản việc chứng minh)

Một thuộc tính cần chú ý của forward process là cho phép ta lấy mẫu $\textbf{x}_t$ tại timestep $t$ bất kỳ thuộc tiến trình, khi đó:
{: .text-justify}

$$q(\textbf{x}_{t}|\textbf{x}_0) = \mathcal{N}\left(\textbf{x}_t;\sqrt{\prod_{s=1}^t(1-\beta_s)}\textbf{x}_0,\left(1-\prod_{s=1}^{t}(1-\beta_s)\right)\mathbf{I}\right)$$

$$q(\textbf{x}_{t}|\textbf{x}_0) = \mathcal{N}\left(\textbf{x}_t;\sqrt{\bar{\alpha}_t}\textbf{x}_0,\left(1-\bar{\alpha}_t\right)\mathbf{I}\right)$$

trong đó $$\bar{\alpha}_t = \prod_{s=1}^t(1-\beta_s)$$. Ta có thể lấy mẫu $\textbf{x}_t$ thuộc phân phối Gaussian có 
$\mu = \sqrt{\bar{\alpha}_t}\textbf{x}_0$ và $\sigma^2 = (1-\bar{\alpha}_t)\mathbf{I}$:
{: .text-justify}

$$\textbf{x}_t = \sqrt{\bar{\alpha}_t}\textbf{x}_0 + \sqrt{1-\bar{\alpha}_t}\epsilon, \ \ \epsilon \sim \mathcal{N}(0,\mathbf{I}) \tag{*}\label{0} $$


Trong quá trình huấn luyện, mô hình sẽ học cách đảo ngược quá trình diffusion trên để tạo ra ảnh mới.
Bắt đầu với Gaussian noise thuần túy $$p(\textbf{x}_{T}) = \mathcal{N}(\textbf{x}_T; \textbf{0}, \textbf{I})$$, mô hình 
sẽ học được phân phối $p_\theta(\textbf{x}_{0:T})$ theo:
{: .text-justify}

$$
\begin{aligned}
p_\theta(\textbf{x}_{0:T}) &= p(\textbf{x}_T)\prod_{t=1}^{T}p_\theta(\textbf{x}_{t-1}|\textbf{x}_t) \\
&= p(\textbf{x}_T)\prod_{t=1}^{T}\mathcal{N}\big(\textbf{x}_{t-1};\mu_\theta(\textbf{x}_t, t), \Sigma_\theta(\textbf{x}_t, t)\big)
\end{aligned}
$$

Những tham số phụ thuộc theo thời gian của Gaussian transition sẽ được mô hình huấn luyện và phân phối reverse diffusion 
transition chỉ phụ thuộc vào timestep trước vì tính chất của Markov.
{: .text-justify}

#### Criterion

Mô hình Diffusion được huấn luyện nhằm tìm ra reverse Markov transition tối đa hóa likelihood của training data. Trong 
thực tế điều này tương đương với việc tối thiểu hóa giới hạn trên của âm ($-$) log likelihood.
{: .text-justify}

$$\mathbb{E}\big[-\log p_\theta(\textbf{x}_0)\big] \leq \mathbb{E}_q\bigg[-\log \frac{p_\theta(\textbf{x}_{0:T})}{q(\textbf{x}_{1:T}|\textbf{x}_0)}\bigg] = L_{vlb}$$

*($L_{vlb}$ là giới hạn trên cần tối thiểu, cũng chính là âm của [Evidence lower bound - ELBO](https://en.wikipedia.org/wiki/Evidence_lower_bound))*
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

Sử dụng Bayes'rule:

$$ q(\textbf{x}_{t-1}|\textbf{x}_t,\textbf{x}_0) = \frac{q(\textbf{x}_t|\textbf{x}_{t-1})q(\textbf{x}_{t-1}|\textbf{x}_0)}{q(\textbf{x}_t|\textbf{x}_0)} $$

$$
\Rightarrow
\begin{aligned}
- \sum_{t=2}^{T} \log \frac{p_\theta(\textbf{x}_{t-1}|\textbf{x}_t)}{q(\textbf{x}_t|\textbf{x}_{t-1})} &= 
- \sum_{t=2}^{T} \log \frac{p_\theta(\textbf{x}_{t-1}|\textbf{x}_t)q(\textbf{x}_{t-1}|\textbf{x}_0)}
{q(\textbf{x}_{t-1}|\textbf{x}_t,\textbf{x}_0)q(\textbf{x}_t|\textbf{x}_0)} \\
&= - \sum_{t=2}^{T} \log \frac{p_\theta(\textbf{x}_{t-1}|\textbf{x}_t)}{q(\textbf{x}_{t-1}|\textbf{x}_t,\textbf{x}_0)}
- \sum_{t=2}^{T} \log\frac{q(\textbf{x}_{t-1}|\textbf{x}_0)}{q(\textbf{x}_t|\textbf{x}_0)}
\end{aligned}
\tag{2}\label{2} 
$$
  
Mặt khác:
$$
\begin{aligned}
-\sum_{t=2}^{T} \log\frac{q(\textbf{x}_{t-1}|\textbf{x}_0)}{q(\textbf{x}_t|\textbf{x}_0)} &= 
-\sum_{t=2}^{T} \log q(\textbf{x}_{t-1}|\textbf{x}_0) + \sum_{t=2}^{T} \log q(\textbf{x}_t|\textbf{x}_0) \\
&= -\sum_{t=1}^{T-1} \log q(\textbf{x}_t|\textbf{x}_0) + \sum_{t=2}^{T} \log q(\textbf{x}_t|\textbf{x}_0) \\
&= -\log q(\textbf{x}_1|\textbf{x}_0) + \log q(\textbf{x}_T|\textbf{x}_0)
\end{aligned}
\tag{3}\label{3} 
$$

Từ $\ref{1}$, $\ref{2}$ và $\ref{3}$ ta được:

$$
\begin{aligned}
\mathbb{E}_q\bigg[-\log \frac{p_\theta(\textbf{x}_{0:T})}{q(\textbf{x}_{1:T}|\textbf{x}_0)}\bigg]  &= 
\mathbb{E}_q\bigg[-\log p(\textbf{x}_T) - \log \frac{p_\theta(\textbf{x}_0|\textbf{x}_1)}{q(\textbf{x}_1|\textbf{x}_0)}
- \sum_{t=2}^{T} \log \frac{p_\theta(\textbf{x}_{t-1}|\textbf{x}_t)}{q(\textbf{x}_{t-1}|\textbf{x}_t,\textbf{x}_0)}
-\log q(\textbf{x}_1|\textbf{x}_0) + \log q(\textbf{x}_T|\textbf{x}_0)
  \bigg] \\
&= \mathbb{E}_q\bigg[-\log \frac{p(\textbf{x}_T)}{q(\textbf{x}_T|\textbf{x}_0)}
-\sum_{t=2}^{T} \log \frac{p_\theta(\textbf{x}_{t-1}|\textbf{x}_t)}{q(\textbf{x}_{t-1}|\textbf{x}_t,\textbf{x}_0)}
-\log p_\theta(\textbf{x}_0|\textbf{x}_1) \bigg]
\end{aligned}
$$

$$
\Rightarrow L_{vlb} = \underbrace{D_{KL}(q(\textbf{x}_T|\textbf{x}_0)\|p(\textbf{x}_T))}_{L_T}
+ \sum_{t=2}^{T}\underbrace{D_{KL}(q(\textbf{x}_{t-1}|\textbf{x}_t,\textbf{x}_0)\|p_\theta(\textbf{x}_{t-1}|\textbf{x}_t))}_{L_{t-1}} \ 
+ \underbrace{\mathbb{E}_q\left[-\log p_\theta(\textbf{x}_0|\textbf{x}_1)\right]}_{L_0}
  \tag{4}\label{4} 
$$
  
Phương trình $\ref{4}$ có $L_{t-1}$ sử dụng $D_{KL}$ để so sánh trực tiếp $p_\theta(\textbf{x}_{t-1}|\textbf{x}_t))$ với 
hậu nghiệm của forward process. Hậu nghiệm này là trackable do có điều kiện trên $\textbf{x}_0$:
{: .text-justify}

$$
q(\textbf{x}_{t-1}|\textbf{x}_t,\textbf{x}_0) = \mathcal{N}(\textbf{x}_{t-1};\tilde{\mu}_t(\textbf{x}_t,\textbf{x}_0), \tilde{\beta}_t\mathbf{I})
$$

với $$\tilde{\mu}_t(\textbf{x}_t,\textbf{x}_0) = \frac{\sqrt{\bar{\alpha}_t-1}\beta_t}{1-\bar{\alpha}_t}\textbf{x}_0 + \frac{\sqrt{\bar{\alpha}_t}(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t}\textbf{x}_t$$ 
và $$ \tilde{\beta}_t = \frac{1-\bar{\alpha}_{t-1}}{1-\bar{\alpha}_t}\beta_t$$.

Kết luận, tất cả KL-divergence trong $\ref{4}$ đều so sánh giữa hai Gaussian với nhau, do đó có thể tính toán bằng 
Rao-Blackwell dưới biểu thức dạng đóng thay vì ước lượng high variance Monte Carlo.
{: .text-justify}

## Diffusion model

Mô hình Diffusion là một dạng mô hình latent variable có class giới hạn, nhưng rất tự do/linh hoạt. Đối với forward 
process, cần phải chọn variance schedule $\beta_t$ thích hợp (nhìn chung giá trị của variance thường có xu hướng tăng). 
Còn reverse process ta sẽ chọn kiến trúc mạng và Gaussian distribution paramaterization. Ràng buộc duy nhất là kiến trúc 
mạng đáp ứng input và output có chung kích thước.
{: .text-justify}

#### Forward process and $L_T$

Theo tác giả paper [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239) chúng ta có thể bỏ qua việc 
variance là tham số có thể được huấn luyện (như đề cập phần *Diffusion theory*). Thay vào đó, variance schedule được đặt 
thành tập những hằng số time-dependent. Ví dụ một linear schedule từ $\beta_1 = 10^{-4}$ đến $\beta_T = 0.2$,...
{: .text-justify}

Điều này dẫn đến xấp xỉ hậu nghiệm của $q$ không có learnable parameter nào, $L_T$ trở thành hằng số (bất kể variance schedule được chọn là gì) 
trong qua trình huấn luyện và có thể bỏ qua.
{: .text-justify}

#### Reverse process and $L_{1:T-1}$

Chúng ta cần lựa chọn Gaussian distribution parameter cho reverse Markov transitions:

$$p_\theta(x_{t-1}|x_t) = \mathcal{N}\left(\textbf{x}_{t-1};\mu_\theta(\textbf{x}_t, t), \Sigma_\theta(\textbf{x}_t, t)\right)$$

Trong đó cần định nghĩa dạng hàm số của $\mu_\theta$ hoặc $\Sigma_\theta$. Có nhiều cách để chọn hàm $\Sigma_\theta$, 
phức tạp như của [Alex Nichol, Prafulla Dhariwal](https://arxiv.org/abs/2102.09672) hay đơn giản của [J. Ho](https://arxiv.org/abs/2006.11239):
{: .text-justify}

$$\Sigma_\theta(x_t, t) = \sigma_t^2\mathbf{I} \\
\sigma_t^2 = \beta_t$$

Điều này nghĩa là ta đã giả định rằng Gaussian đa biến là tích vô hướng của các Gaussian độc lập với variance phụ thuộc 
thời gian xác định. Đặt những variance này theo variance schedule của forward process. Từ đây ta có:
{: .text-justify}

$$p_\theta(x_{t-1}|x_t) = \mathcal{N}\left(\textbf{x}_{t-1};\mu_\theta(\textbf{x}_t, t), \sigma_\theta^2\mathbf{I}\right)$$

Nên có thể viết:

$$L_{t-1} = \mathbb{E}_q\left[\frac{1}{2\sigma_t^2}\lVert \tilde{\mu}_t(\textbf{x}_t,\textbf{x}_0) - \mu_\theta(\textbf{x}_t,t)\rVert^2\right] + C$$

Với $C$ là hằng số không phụ thuộc vào $\theta$. Tham số đơn giản nhất có thể chọn được của $\mu_\theta$ là một mô hình dự đoán 
$\tilde{\mu}_t$ tức giá trị trung bình hậu nghiệm của forward process. Tuy nhiên $\tilde{\mu}_t$ có chứa yếu tố ngẫu nhiên 
(do mỗi quá trình chuyển tiếp được gán thêm gaussian noise ngẫu nhiên) ẩn bên trong, do đó muốn backpropagation hiệu quả 
cần làm rõ yếu tố ngẫu nhiên này. Một cách tiếp cận là thực hiện reparameterization, từ $\ref{0}$ viết lại $\tilde{\mu}_t$ 
dưới dạng:
{: .text-justify}

$$\tilde{\mu}_t = \frac{1}{\sqrt{\alpha_t}}\left(\textbf{x}_t - \frac{\beta_t}{\sqrt{1-\bar{\alpha}_t}}\epsilon\right), 
\ \ \epsilon \sim \mathcal{N}(0,\mathbf{I})$$

Theo đó tác giả J. Ho đưa ra việc huấn luyện $\mu_\theta$ để dự đoán noise component tại mỗi timestep bất kỳ:
{: .text-justify}

$$\mu_\theta(\textbf{x}_t, t) = \frac{1}{\sqrt{\alpha_t}}\left(\textbf{x}_t - \frac{\beta_t}{\sqrt{1-\bar{\alpha}_t}}\epsilon_\theta(\textbf{x}_t, t)\right)$$

điều này dẫn đến một hàm loss thay thế mà theo đánh giá của J. Ho là ổn định và cho kết quả tốt:

$$L_{simp}(\theta) = \mathbb{E}_{t,\textbf{x}_{0},\epsilon}\left[\lVert \epsilon - \epsilon_\theta\left(\sqrt{\bar{\alpha}}_t\textbf{x}_{0} + \sqrt{1-\bar{\alpha}_t}\epsilon,t\right) \rVert^2\right]$$ 

Tác giả cũng lưu ý các mối liên hệ giữa công thức của Diffusion model với *Score-matching generative model* dựa trên 
Langevin dynamic. Theo đó Diffusion model và Score-based model giống như sự phát triển của cơ học lượng tử wave-based 
và cơ học lượng tử matrix-based - phát hiện ra hai công thức tương đương của cùng một hiện tượng.
{: .text-justify}

Để xác định kiến trúc mô hình $\epsilon_\theta$, ta nhắc lại điều kiện ràng buộc (duy nhất): input và output có cùng 
kích thước. Mô hình kiến trúc dạng U-Net thường được sử dụng cho mục tiêu này.
{: .text-justify}

#### Reverse process decoder and $L_0$

Tại bước cuối cùng của chuỗi các biển đổi theo conditional Gaussian distribution liên tục, cần đạt được mục tiêu sinh ảnh 
giống với data distribution. Do đó cần tìm cách có được discrete (log) likelihood cho mỗi pixel trên toàn bộ kích thước ảnh. 
Bước cuối của reverse diffusion chain được đặt thành một decoder độc lập rời rạc. Để xác định likelihood của ảnh $\textbf{x}_0$ 
cho trước trên $\textbf{x}_1$, bước đầu tiên ta phải ép điều kiện độc lập giữa các chiều dữ liệu:
{: .text-justify}

$$p_\theta(\textbf{x}_0|\textbf{x}_1) = \prod_{i=1}^D p_\theta(\textbf{x}_0^i|\textbf{x}_1^i)$$

$D$ là kích thước của dữ liệu và chỉ số $i$ là chỉ số trên mỗi chiều. Ta có thể dùng ma trận hiệp phương sai để biến đổi 
công thức của Gaussian đa biến thành tích của các Gaussian đơn biến theo kích thước dữ liệu:
{: .text-justify}

$$\mathcal{N}\left(\textbf{x};\mu_\theta(\textbf{x}_1, 1), \sigma_\theta^2\mathbf{I}\right) = \prod_{i=1}^D\mathcal{N}\left(\textbf{x};\mu_\theta^i(\textbf{x}_1,1),\sigma_1^2\right)$$

Mục tiêu là xác định giá trị số nguyên với mỗi pixel cho trước và điều kiện đã biết là phân phối các possible value cho 
pixel tương ứng trên ảnh nhiễu tại thời điểm $t = 1$:
{: .text-justify}

$$\mathcal{N}(\textbf{x};\mu_\theta^i(\textbf{x}_1,1),\sigma_1^2)$$

Giả sử ảnh bao gồm các giá trị nguyên $0, 1, ...,255$ (ảnh RGB thông thường), được scale tuyến tính về $[-1, 1]$. Xác 
suất pixel có giá trị $x$ trên phân phối Gaussian đơn biến của vị trí pixel đó tại $\textbf{x}_1$ được tính theo: 
{: .text-justify}

$$\int_{\delta_{-}(x)}^{\delta_+(x)}\mathcal{N}\left(x;\mu_\theta(\textbf{x}_1, 1), \sigma_\theta^2\right) \ dx$$

$$ 
\begin{split}
\delta_+(x) =
\begin{cases}
+\infty  & \text{if $x=1$} \\
x+\frac{1}{255} & \text{if $x\lt1$}
\end{cases}
\end{split}
\ \ \ \ \ \ \ 
\begin{split}

\delta_-(x) =
\begin{cases}
-\infty  & \text{if $x=-1$} \\
x-\frac{1}{255} & \text{if $x\gt-1$}
\end{cases}
\end{split}
$$

Công thức trên toàn kích thước ảnh:

$$p_\theta(\textbf{x}_0|\textbf{x}_1) = \prod_{i=1}^{D}\int_{\delta_{-}(x_0^i)}^{\delta_+(x_0^i)}\mathcal{N}\left(x;\mu_\theta^i(\textbf{x}_1, 1), \sigma_\theta^2\right) \ dx$$

## Training and Sampling

Từ hàm mục tiêu đã được đơn giản hóa như ở trên:

$$L_{simp}(\theta) = \mathbb{E}_{t,\textbf{x}_{0},\epsilon}\left[\lVert \epsilon - \epsilon_\theta\left(\sqrt{\bar{\alpha}}_t\textbf{x}_{0} + \sqrt{1-\bar{\alpha}_t}\epsilon,t\right) \rVert^2\right]$$ 

Việc huấn luyện và lấy mẫu được mô tả như sau:

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-1-3.png){:style="display:block; margin-left:auto; margin-right:auto"}


<div align="center">.</div> 

---