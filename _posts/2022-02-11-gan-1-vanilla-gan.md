---
layout: posts
title: "Generative Adversarial Networks I: Vanilla GAN"
categories: generative-adversarial-networks
mathjax: true
header:
  image: /assets/images/gan.jpeg
  image_description: "Creative"
---

*Note mở đầu trong chuỗi notepage về **Generative Adversarial Network (GAN) và các biến thể** sẽ đề cập đến mô hình gốc của GAN. Giống như các note khác, 
nội dung chủ yếu đi vào các chi tiết quan trọng, cần nhớ (với bản thân mình) thay vì nói về nguồn gốc, giới thiệu.*

---

### Generative Adversarial Networks

Paper [Generative Adversarial Network](https://arxiv.org/abs/1406.2661) (2014), Ian J. Goodfellow.

#### Generative Model

Mô hình GAN được xếp vào nhóm mô hình *Generative* so với nhóm mô hình *Discriminative*. Các mô hình *discriminative* sẽ đưa ra 
những dự đoán về nhãn hoặc giá trị $ y $ từ biến đầu vào $ \mathbf{x} $, giá trị dự báo là một xác suất có điều kiện $ \mathrm{P}(y|\mathbf{x}) $ .
 Trong đó $ y $ là mục tiêu cần dự báo và $ \mathbf{x} $ là điều kiện. Hàm số *sigmoid* và *softmax* thường được sử dụng để dự báo xác suất. 
Ví dụ trong trường hợp dự báo nhị phân:

$$ p(y|\mathbf{x}) = \frac{1}{1+e^{-w^T\mathbf{x}}} $$

Ngược lại, mô hình *generative* dự báo $ \mathrm{P}(\mathbf{x}\mid y) $ tức là dựa vào đầu ra mong muốn để tìm kiếm các đặc trưng của dữ liệu. 
Dựa vào *công thức bayes* để tính ngược lại xác suất $ \mathrm{P}(y|\mathbf{x}) $:

$$ \begin{aligned} p(y|\mathbf{x}) &= \frac{p(\mathbf{x},y)}{p(\mathbf{x})} \\ &= \frac{p(\mathbf{x}|y)p(y)}{\sum_{y}{p(\mathbf{x},y)}} \\ &= \frac{p(\mathbf{x}|y)p(y)}{\sum_{y}{p(\mathbf{x}|y)p(y)}} \end{aligned} $$

#### Implicit Model

Hai dạng chính của *Generative model* là *Explicit* và *Implicit*.

Các mô hình *explicit* dùng một hàm phân phối xác suất chọn trước, sau đó tối ưu tham số phân phối phù hợp với bộ dữ liệu huấn luyện thông qua 
*hàm hợp lý cực đại (maximum likelihood function)*. Dữ liệu mới sau này sẽ được sinh ngẫu nhiên từ phân phối xác suất đó.

Mô hình *implicit* không chọn trước một hàm phân phối, khả năng sinh dữ liệu giống với dữ liệu thật hoàn toàn được học thông qua quá trình huấn luyện. 
Phương pháp này ước lượng tham số của phân phối tiền định mà không dùng *maximum likelihood estimation* để mô phỏng phân phối. Do đó dữ liệu được sinh 
trực tiếp từ mô hình.

GAN là một *implicit model*. Đặc biệt, GAN sử dụng *supervised learning* để giải quyết vấn đề *unsupervised learning*, 
điều này được thực hiện thông qua hai thành phần đối lập *Discriminator* và *Generator* 

![Kiến trúc của GAN]({{ site.url }}{{ site.baseurl }}/assets/images/posts/g1-structure.jpg){:style="display:block; margin-left:auto; margin-right:auto"}

Hai thành phần (có thể coi như hai model) generator và discriminator được huấn luyện đồng thời. Trong khi generator có nhiệm vụ sinh ảnh thì discriminator 
lại có chức năng phân biệt ảnh thật hay giả. Đầu vào của discriminator là ảnh sinh từ generator và ảnh thật từ dữ liệu.

Cả discriminator và generator đều được tối ưu để thực hiện tốt nhất nhiệm vụ của mình, mô hình GAN được cho là hội tụ khi discriminator không thể đánh giá 
được ảnh sinh là thật hay giả.

### Loss function

Hàm loss của GAN có mục đích kết hợp tối ưu mục tiêu của cả Discriminator và Generator

$$ \underset{G}{\min}\underset{D}{\max}V(D,G) = \mathbb{E}_{x \sim p_{data}(x)}[\log D(x)] + \mathbb{E}_{z \sim p_{z}(z)}[\log (1-D(G(z)))] \tag{1}\label{1} $$

Giờ ta sẽ làm rõ ý nghĩa của hàm loss này!

Ta có $ D(y) $ là xác suất y nằm trong plausible data. Giả sử:

$$ \begin{cases} 
L(y) = 1, & \text{nếu y là ảnh thật} \\
L(y) = 0, & \text{nếu y là ảnh giả}
\end{cases} $$

Công thức cross-entropy:

$$ -(L(y)\log (D(y)) + (1 - L(y))\log (1-D(y))) \tag{2}\label{2}$$

Biết rằng
$$ \begin{cases} 
L(x) = 1, & \text{với mọi } x \sim p_{data}(x)\\
L(y) = 0, & \text{với mọi } z \sim p_{z}(z) 
\end{cases} $$
, vì thế:

 - đối với ảnh thật $ x \sim p_{data}(x) $: $ \eqref{2} \approx -\log (D(x)) $
 - đối với ảnh sinh $ z \sim p_{z}(z) $: $ \eqref{2} \approx -\log (1-D(G(z))) $

Đổi dấu hàm trên ta có $ V $
Mục tiêu huấn luyện $ D $ là tối thiểu cross-entropy, tương đương với tối đa $ V $, ngược lại mục tiêu huấn luyện $ G $ để đánh lừa discriminator, 
tương đương với việc tối thiểu $ V $.

