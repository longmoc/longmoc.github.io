---
layout: single
title: "Latent Diffusion Models"
categories: diffusion-models
mathjax: true
header:
  image: /assets/images/gan.jpeg
  image_description: "Creative"
  teaser: /assets/images/gan.jpeg
---

*Diffusion models (DMs) đã thay thế GANs trở thành mô hình sinh ảnh hiệu quả, đặc biệt với ảnh có độ phân giải cao. 
Song mô hình Diffusion gốc vẫn tồn tại hạn chế. Một trong số đó là việc các biến đổi hoạt động trực tiếp trên pixel 
space, dẫn đến yêu cầu về resource quá lớn và mất nhiều thời gian.*
{: .text-justify}

*Latent Diffusion Models (LDMs) thực thi các thành phần diffusion trên latent space từ pretrained autoencoder để giảm 
thiểu hạn chế kể trên*.
{: .text-justify}

---

## Latent Representations

*LDMs* sử dụng các perceptual compression model đã được huấn luyện $\mathcal{E}$ và $\mathcal{D} để đưa ảnh từ pixel space về low-dimensional latent space hiệu quả hơn. So sánh với high-dimensional pixel space thì latent space phù hợp với loại 
mô hình sinh likelihood-based:
{: .text-justify}
- Tập trung vào những phần quan trọng, có ý nghĩa của dữ liệu
- Hoạt động huấn luyện trên low-dimensional cần ít chi phí tính toán hơn.

Không những giúp tốc độ sinh nhanh hơn, giảm yêu cầu resource mà còn cho phép thực hiện nhiều tác vụ hơn. Việc nén dữ 
liệu đầu vào về latent space có thể coi như hành động encode, dó đó có thể feed nhiều loại input khác nhau như ảnh hoặc 
text. Model sẽ học để mã hóa các loại dữ liệu này về cùng một sub-space mà diffusion model sử dụng để sinh dữ liệu. Vì 
thế ta có thể sử dụng văn bản để sinh ảnh theo chỉ dẫn.
{: .text-justify}

![]({{ site.url }}{{ site.baseurl }}/assets/images/posts/diffusion-2-1.png){:style="display:block; margin-left:auto; margin-right:auto"}

Nhắc lại *DMs*, mục tiêu huấn luyện có thể viết dưới dạng đơn giản:
{: .text-justify}

$$L_{DM} = \mathbb{E}_{\textbf{x}_t,\epsilon \sim \mathcal{N}(0,\mathbf{I}),t}\left[\lVert\epsilon - \epsilon_\theta(\textbf{x}_t,t)\rVert^2_2\right]$$

Theo đó, mục tiêu huấn luyện của *LMDs*:
{: .text-justify}

$$L_{LDM} = \mathbb{E}_{\varepsilon(\textbf{x}),\epsilon \sim \mathcal{N}(0,\mathbf{I}),t}\left[\lVert\epsilon - \epsilon_\theta(\textbf{z}_t,t)\rVert^2_2\right]$$

<div align="center">.</div> 

---