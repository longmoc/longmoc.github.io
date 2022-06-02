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

--

Tác giả của paper [Wasserstein GAN](https://arxiv.org/abs/1701.07875) - Arjovsky đã nêu ra những hạn chế của GAN thông 
thường và đề xuất một biến thể mang tên Wasserstein GAN.

## Hạn chế của Vanilla GAN Loss

Một vấn đề thường thấy khi huấn luyện các mô hình GAN với GAN Loss là ở một giai đoạn nhất định *discriminator* thực 
hiện tốt khả năng phân biệt nhưng *generator* lại không đủ tốt dẫn đến gradient của *generator* bị triệt tiêu và không 
học được gì.

$$ -\nabla_{}\log (1-D(G(z))) \to 0 $$

Ở bài báo gốc tác giả cũng lường trước vấn đề này nên đề xuất một hàm loss thay thế. Tuy nhiên gradient của hàm này lại 
có phương sai rất lớn làm quá trình huấn luyện không ổn định (*unstable*)

$$ \nabla_{}\log D(G(z)) \to \text{large variance of gradient} $$ 


## Wasserstein GAN


<div align="center">.</div> 

---