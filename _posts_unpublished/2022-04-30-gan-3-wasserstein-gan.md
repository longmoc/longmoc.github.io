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

*Note tiếp theo trong series **Generative Adversarial Network (GAN) và các biến thể**. Nội dung phần này giống như tiêu đề 
bài note sẽ đề cập đến Wasserstein-GAN và ý nghĩa đối với những hạn chế của mô hình GAN thông thường*.

---
## Hạn chế của Vanilla GAN Loss
 
Một vấn đề thường thấy khi huấn luyện các mô hình GAN với GAN Loss là ở một giai đoạn nhất định *discriminator* thực 
hiện tốt khả năng phân biệt nhưng *generator* lại không đủ tốt dẫn đến gradient của *generator* bị triệt tiêu và không 
học được gì.

$$ \nabla_{}\log (1-D(G(z))) \to 0 $$

## Wasserstein GAN


<div align="center">.</div> 

---