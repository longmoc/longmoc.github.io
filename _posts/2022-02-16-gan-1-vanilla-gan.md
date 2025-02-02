---
layout: single
title: "Generative Adversarial Networks I: Vanilla GAN"
categories: generative-adversarial-networks
mathjax: true
header:
  image: /assets/images/gan.jpeg
  image_description: "Creative"
  teaser: /assets/images/gan.jpeg
---

*Note mở đầu trong chuỗi notepage về **Generative Adversarial Network (GAN) và các biến thể** sẽ đề cập đến mô hình gốc của GAN. Giống như các note khác, 
nội dung chủ yếu đi vào các chi tiết quan trọng cần nhớ (với bản thân mình) thay vì nói về nguồn gốc, giới thiệu.*
{: .text-justify}

---

## Generative Adversarial Networks

Paper [Generative Adversarial Network](https://arxiv.org/abs/1406.2661) (2014), Ian J. Goodfellow.
{: .text-justify}

### Generative Model

Mô hình GAN được xếp vào nhóm mô hình *Generative* so với nhóm mô hình *Discriminative*. Các mô hình *discriminative* sẽ đưa ra 
những dự đoán về nhãn hoặc giá trị $ y $ từ biến đầu vào $ \mathbf{x} $, giá trị dự báo là một xác suất có điều kiện $ \mathrm{P}(y|\mathbf{x}) $ .
 Trong đó $ y $ là mục tiêu cần dự báo và $ \mathbf{x} $ là điều kiện. Hàm số *sigmoid* và *softmax* thường được sử dụng để dự báo xác suất. 
Ví dụ trong trường hợp dự báo nhị phân:
{: .text-justify}

$$ p(y|\mathbf{x}) = \frac{1}{1+e^{-w^T\mathbf{x}}} $$

Ngược lại, mô hình *generative* dự báo $ \mathrm{P}(\mathbf{x}\mid y) $ tức là dựa vào đầu ra mong muốn để tìm kiếm các đặc trưng của dữ liệu. 
Dựa vào *công thức bayes* để tính ngược lại xác suất $ \mathrm{P}(y|\mathbf{x}) $:
{: .text-justify}

$$ \begin{aligned} p(y|\mathbf{x}) &= \frac{p(\mathbf{x},y)}{p(\mathbf{x})} \\ &= \frac{p(\mathbf{x}|y)p(y)}{\sum_{y}{p(\mathbf{x},y)}} \\ &= \frac{p(\mathbf{x}|y)p(y)}{\sum_{y}{p(\mathbf{x}|y)p(y)}} \end{aligned} $$

### Implicit Model

Hai dạng chính của *Generative model* là *Explicit* và *Implicit*.
{: .text-justify}

Các mô hình *explicit* dùng một hàm phân phối xác suất chọn trước, sau đó tối ưu tham số phân phối phù hợp với bộ dữ liệu huấn luyện thông qua 
*hàm hợp lý cực đại (maximum likelihood function)*. Dữ liệu mới sau này sẽ được sinh ngẫu nhiên từ phân phối xác suất đó.
{: .text-justify}

Mô hình *implicit* không chọn trước một hàm phân phối, khả năng sinh dữ liệu giống với dữ liệu thật hoàn toàn được học thông qua quá trình huấn luyện. 
Phương pháp này ước lượng tham số của phân phối tiền định mà không dùng *maximum likelihood estimation* để mô phỏng phân phối. Do đó dữ liệu được sinh 
trực tiếp từ mô hình.
{: .text-justify}

GAN là một *implicit model*. Đặc biệt, GAN sử dụng *supervised learning* để giải quyết vấn đề *unsupervised learning*, 
điều này được thực hiện thông qua hai thành phần đối lập *Discriminator* và *Generator* 
{: .text-justify}

![Kiến trúc của GAN]({{ site.url }}{{ site.baseurl }}/assets/images/posts/g1-structure.jpg){:style="display:block; margin-left:auto; margin-right:auto"}

Hai thành phần (có thể coi như hai model) *generator* và *discriminator* được huấn luyện đồng thời. Trong khi *generator* có nhiệm vụ sinh ảnh thì *discriminator*
lại có chức năng phân biệt ảnh thật hay giả. Đầu vào của *discriminator* là ảnh sinh từ *generator* và ảnh thật từ dữ liệu.
{: .text-justify}

Cả *discriminator* và *generator* đều được tối ưu để thực hiện tốt nhất nhiệm vụ của mình, mô hình GAN được cho là hội tụ khi *discriminator* không thể đánh giá 
được ảnh sinh là thật hay giả.
{: .text-justify}

## GANs Training

Hàm loss của GAN có mục đích kết hợp tối ưu mục tiêu của cả *discriminator* và *generator*
{: .text-justify}

$$ \underset{G}{\min}\underset{D}{\max}V(D,G) = \mathbb{E}_{x \sim p_{data}(x)}[\log D(x)] + \mathbb{E}_{z \sim p_{z}(z)}[\log (1-D(G(z)))] \tag{1}\label{1} $$

Giờ ta sẽ làm rõ ý nghĩa của hàm loss này cũng như quá trình huấn luyện.
{: .text-justify}

### Discriminator loss function 

Ta có $ D(y) $ là xác suất *discriminator* đánh giá $ y $ nằm trong plausible data, $ L(y) $ xác suất $ y $ là ảnh thật trong thực tế. 
Định lượng thông tin cần thiết để biến đổi từ phân phối của L sang D được tính theo công thức của cross-entropy:
{: .text-justify}

$$ H(L,D) = -\mathbb{E}_L[\log (D(x))] = -\sum_{i} L(y_i)\log (D(y_i) $$

Xét trên phân phối xác suất của $ y $ là ảnh sinh $ L^{\'}(y) = 1 - L(y) $, xác suất đánh giá của *discriminator* là $ (1-D(y)) $.
{: .text-justify}

$$ H(L^{'},D) = -\sum_{i} 1-L(y_i)\log (1-D(y_i) $$

Trong quá trình huấn luyện *discriminator*, dữ liệu ảnh thật ($ x \sim p_{data} $) và dữ liệu ảnh sinh ($ \hat{x} \sim p_g $) được đưa vào đồng thời. Với
{: .text-justify}

$$ \begin{cases} 
L(x) = 1, & x \sim p_{data} \\
L(\hat{x}) = 0, & \hat{x} \sim p_g
\end{cases} $$

Kết hợp lại hàm loss của *disciminator* có dạng:
{: .text-justify}

$$ \begin{aligned} L_D &= -\sum_{i} \log (D(x_i) - \sum_{i} log (1 - D(\hat{x_i})) \\
&= - \bigl[\mathbb{E}_{x \sim p_{data}}[\log D(x)] + \mathbb{E}_{\hat{x} \sim p_{g}}[\log (1-D(\hat{x}))]\bigr] 
\end{aligned} $$

Ảnh sinh được lấy mẫu từ $ p_g $ là output của *generator* với input từ *latent space* $ z $, 
do đó có thể viết $ D(\hat{x}), \hat{x} \sim p_g $ thành $ D(G(z)), z \sim p_z $
{: .text-justify}

$$ L_D = - \bigl[\mathbb{E}_{x \sim p_{data}}[\log D(x)] + \mathbb{E}_{z \sim p_{z}}[\log (1-D(G(z)))]\bigr] $$

### Zero-sum game

Mục tiêu huấn luyện $ D $ là tối thiểu $ L_D $, tương đương với tối đa $ -L_D $
{: .text-justify}

$$ \underset{D}{\max}\bigl[-L_D \bigr] = \underset{D}{\max}\bigl[\mathbb{E}_{x \sim p_{data}}[\log D(x)] + \mathbb{E}_{z \sim p_{z}}[\log (1-D(G(z)))]\bigr] \tag{2}\label{2}$$

Ngược lại mục tiêu huấn luyện $ G $ để đánh lừa discriminator, tương đương với tối thiểu $ -L_D $ nhưng chỉ xét trên tập ảnh sinh. 
{: .text-justify}

$$ \underset{G}{\min}\bigl[-L_D \bigr] = \underset{G}{\min}\bigl[\mathbb{E}_{z \sim p_{z}}[\log (1-D(G(z)))]\bigr] \tag{3}\label{3}$$

Và vì việc huấn luyện *generator* và *discriminator* là đồng thời, liên quan chặt chẽ với nhau nên có thể mô tả 
*Zero-sum* game thông qua **value function** $ V(D,G) = -L_D $. 
Bài toán lúc này trở thành bài toán tối ưu $$ \underset{G}{\min}\underset{D}{\max}V(D,G)] $$ như công thức của $ \eqref{1} $.
{: .text-justify}

Việc cập nhật trọng số của *discriminator* và *generator* được mô tả trong paper thể hiện cho $ \eqref{2} $ và $ \eqref{3} $, 
với cost được normalize theo batch (chia cho $ m $).
{: .text-justify}

![Cập nhật trọng số D và G]({{ site.url }}{{ site.baseurl }}/assets/images/posts/g1-training.png){:style="display:block; margin-left:auto; margin-right:auto"}

Ta thấy các công thức này khá giống cross-entropy trên *Bernoulli distribution* hay ***Binary Cross-Entropy***.
{: .text-justify}

### Điểm hội tụ

Tại điểm mô hình GAN hội tụ, giả sử nghiệm hội tụ của *generator* là $ G^* $, khi đó $ G^* \rightarrow x $ và 
$$ \mathbb{E}_{z \sim p_{z}(z)}[f(G^{*}(z))] \rightarrow \mathbb{E}_{x \sim p_{g}(x)}[f(x)] $$. Suy ra
{: .text-justify}

$$ \eqref{1} \Leftrightarrow \underset{D}{\max}V(D,G^*) = \mathbb{E}_{x \sim p_{data}(x)}[\log D(x)] + \mathbb{E}_{x \sim p_{g}(x)}[\log (1-D(x))] \tag{4}\label{4} $$

Mặt khác input noise $ z \sim p_{z}(z) $ là ngẫu nhiên $ \Rightarrow $ giá trị hàm generator có thể coi là hàm liên tục. Tương tự x cũng liên tục
{: .text-justify}

$$ \mathbb{E}_{x \sim p(x)}[f(x)] = \int_{x}{p(x)f(x) \ dx} \tag{5}\label{5} $$

$$ \begin{aligned} \eqref{4}, \eqref{5} \Rightarrow \underset{D}{\max}V(D,G^{*}) &= \int_{x}{p_{data}(x)\log (D(x)) \ dx} + \int_{x}{p_{g}(x)\log (1-D(x)) \ dx} \\
&= \int_{x}{\biggl(p_{data}(x)\log (D(x)) + p_{g}(x)\log (1-D(x))\biggr) \ dx}  
\end{aligned} $$

Đặt $ D(x) = y $. Xét $ V(y) = p_{data}(x)\log (y) + p_{g}(x)\log (1-y) $ là hàm khả vi 2 lần:
{: .text-justify}

$$ \begin{cases} 
\frac{\partial V}{\partial y} = \frac{p_{data}(x)}{y} - \frac{p_g(x)}{1-y} \\
\frac{\partial ^2V}{\partial y^2} = -\frac{p_{data}(x)}{y^2} - \frac{p_g(x)}{(1-y)^2} \lt 0
\end{cases} $$

$ \Rightarrow V(y) $ là hàm lõm (concave function).
{: .text-justify}

$ \Rightarrow $ Cực đại của $ V(y) $ là nghiệm của phương trình đạo hàm bậc nhất bằng 0
{: .text-justify}

$$ \begin{aligned} &\ \frac{p_{data}(x)}{y} - \frac{p_g(x)}{1-y} &= \ &0 \\
\Leftrightarrow \ \ &\ \frac{p_{data}(x)(1-y) - p_g(x)y}{y(1-y)} &= \ &0 \\
\Rightarrow \ \ &\ p_{data}(x)y + p_g(x)y &= \ &p_{data}(x) \\
\Rightarrow \ \ &\ y &= \ &\frac{p_{data}(x)}{p_{data}(x) + p_g(x)}
\end{aligned} $$

$ \Rightarrow $ Giá trị hội tụ
{: .text-justify}

$$ \begin{aligned} &\int_{x}{\biggl[p_{data}(x)\log \biggl(\frac{p_{data}(x)}{p_{data}(x) + p_g(x)}\biggr) + p_{g}(x)\log \biggl(1-\frac{p_{data}(x)}{p_{data}(x) + p_g(x)}\biggr)\biggr] \ dx} \\
= \ &\int_{x}{\biggl[p_{data}(x)\log \biggl(\frac{p_{data}(x)}{p_{data}(x) + p_g(x)}\biggr) + p_{g}(x)\log \biggl(\frac{p_g(x)}{p_{data}(x) + p_g(x)}\biggr)\biggr] \ dx} \\
= \ &\int_{x}{\biggl[p_{data}(x)\log \biggl(2\frac{p_{data}(x)}{p_{data}(x) + p_g(x)}\biggr) - \log 2p_{data}(x) 
+ p_{g}(x)\log \biggl(2\frac{p_g(x)}{p_{data}(x) + p_g(x)}\biggr) - \log 2p_{data}(x)\biggr] \ dx} \\
= \ &\int_{x}{\biggl[p_{data}(x)\log \biggl(2\frac{p_{data}(x)}{p_{data}(x) + p_g(x)}\biggr) + p_{g}(x)\log \biggl(2\frac{p_g(x)}{p_{data}(x) + p_g(x)}\biggr)\biggr] \ dx} \\
 &- \log 2\int_{x}{p_{data}(x) \ dx} - \log 2\int_{x}{p_g(x) \ dx} \\
= \ &\mathrm{D_{KL}}(P_{data} \| \frac{P_{data} + P_g}{2}) + \mathrm{D_{KL}}(P_{g} \| \frac{P_{data} + P_g}{2}) - 2\log 2 \\
  \\
= \ &\ \ \ \ \bbox[5px,border:2px solid red]{2\cdot\mathrm{D_{JS}}(P_{data} \| P_g) - 2\log 2}   
\end{aligned} $$

Trong đó $ \mathrm{D_{KL}} $ là ***Kullback-Leibler divergence*** và $ \mathrm{D_{JS}} $ ***là Jensen-Shannon divergence***.
{: .text-justify}

Tại $ G^* \rightarrow x $, ảnh thật và ảnh sinh là giống nhau, đồng nghĩa $$ \mathrm{D_{JS}}(P_{data}(x) \| P_g(x)) = 0 $$.
Vậy giá trị hội tụ của loss khi huấn luyện mô hình GAN là $ \bbox[5px,border:2px solid red]{-2\log 2} $.
{: .text-justify}

---
