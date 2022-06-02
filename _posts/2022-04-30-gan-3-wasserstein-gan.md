---
layout: single
title: "Generative Adversarial Networks III: Wasserstein GAN"
categories: generative-adversarial-networks
mathjax: true
header:
  image: /assets/images/gan.jpeg
  image_description: "Creative"
  teaser: /assets/images/gan.jpeg
---

*Note tiếp theo trong series **Generative Adversarial Network (GAN) và các biến thể**. Giống như tiêu đề 
bài note sẽ đề cập đến Wasserstein-GAN - giải quyết những hạn chế của mô hình GAN thông thường.*

*Nội dung phần này có liên quan đến một số note trước đó như Vanilla GAN và đặc biệt là Wasserstein distance*.

---

Tác giả của paper [Wasserstein GAN](https://arxiv.org/abs/1701.07875) - Arjovsky đã nêu ra những hạn chế của GAN thông 
thường và đề xuất một biến thể mang tên Wasserstein GAN.

## Hạn chế của Vanilla GAN Loss

Một vấn đề thường thấy khi huấn luyện các mô hình GAN với GAN Loss là ở một giai đoạn nhất định *discriminator* thực 
hiện tốt khả năng phân biệt nhưng *generator* lại không đủ tốt dẫn đến gradient của *generator* bị triệt tiêu và không 
học được gì.

$$ -\nabla_{\theta_g}\log (1-D(G(z))) \to 0 $$

Ở bài báo gốc tác giả cũng lường trước vấn đề này nên đề xuất một hàm loss thay thế. Tuy nhiên gradient của hàm này lại 
có phương sai rất lớn làm quá trình huấn luyện không ổn định (*unstable*)

$$ \nabla_{\theta_g}\log D(G(z)) \to \text{large variance of gradient} $$ 


## Wasserstein GAN

Để giải quyết những vấn đề trên, Wasserstein GAN (WGAN) đã đưa ra một hàm loss mới sử dụng *Wasserstein distance* có 
gradient mượt và ổn định hơn trong quá trình huấn luyện. 

$$ W(p_r, p_\theta) = \sup_{\Vert f \Vert_L \leq 1} \mathbf{E}_{x \sim p_r}[f(x)]-\mathbf{E}_{x \sim p_{\theta}}[f(x)] $$

Như đã trình bày ở bài note về [Wasserstein distance](https://longmoc.github.io/mathematic/mathematic-2-wasserstein-distance-2/), 
hàm $f$ là một hàm *1-Lipschitz* do đó độ lớn đạo hàm của chênh lệch kỳ vọng giữa phân phối thực và sinh bị chặn trên, 
điều này giúp cho WGAN loss khắc phục được hiện tượng *gradient explosion*. Nhờ vậy mô hình WGAN có thể huấn luyện được 
ngay tại thời điểm *generator* thể hiện không tốt như *discriminator*.

Trong paper WGAN, tác giả giữ nguyên kiến trúc *discriminator* và chỉ loại bỏ hàm *sigmoid* cuối cùng. Khi đó output của 
*discriminator* là output của linear và phải là hàm *1-Lipschitz*. Giá trị này có dạng scalar thay vì xác suất như 
Vanilla GAN, khá tương đồng với *value function* trong Reinforcement learning. Tác giả cũng đổi tên *discriminator* thành 
***critic*** để phản ánh đúng vai trò của nó. Nhãn của mô hình cũng thay đổi từ $0$, $1$ sang $-1$, $1$. 

![GAN]({{ site.url }}{{ site.baseurl }}/assets/images/posts/g2-vanilla-gan.jpeg){:style="display:block; margin-left:auto; margin-right:auto"}
<div align="center"><i>Vanilla GAN</i></div> 



![WGAN]({{ site.url }}{{ site.baseurl }}/assets/images/posts/g2-wgan.jpeg){:style="display:block; margin-left:auto; margin-right:auto"}
<div align="center"><i>WGAN</i></div> 


Để đảm bảo ràng buộc $f$ phải là *1-Lipschitz*, WGAN đơn giản chỉ sử dụng phương pháp clipping làm hạn chế giá trị 
weight tối đa của $f$ sau mỗi batch. Như trong paper tác giả giới hạn *critic* weights trong khoảng nhất định 
thông qua hyperparameters $c$.

WGAN cũng thay đổi optimizer sử dụng phương pháp RMSProp để thực hiện gradient descent với momentum = $0$.

Quá trình huấn luyện WGAN được mô tả như sau:
![Training WGAN]({{ site.url }}{{ site.baseurl }}/assets/images/posts/g2-training.png){:style="display:block; margin-left:auto; margin-right:auto"}



<div align="center">.</div> 

---