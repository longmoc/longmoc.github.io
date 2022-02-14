---
layout: posts
title: "Generative Adversarial Networks I: Vanilla GAN"
categories: generative-adversarial-networks
mathjax: true
header:
  image: /assets/images/gan.jpeg
  image_description: "Creative"
---

Note mở đầu trong chuỗi notepage về * Generative Adversarial Network (GAN) và các biến thể * sẽ đề cập đến mô hình gốc của GAN.

### Generative Adversarial Network

Mô hình GAN được xếp vào nhóm mô hình *Generative* so với nhóm mô hình *Discriminative*. Các mô hình *discriminative* sẽ đưa ra 
những dự đoán về nhãn hoặc giá trị $ y $ từ biến đầu vào $ \mathbf{x} $, giá trị dự báo là một xác suất có điều kiện $ \mathrm{P}(y|\mathbf{x}) $
, bài toán unsupervised

Giải quyết unsupervised bằng supervised

Generator

Discriminator

### Loss function

Hàm loss của GAN có mục đích kết hợp tối ưu mục tiêu của cả Discriminator và Generator

$$ \underset{G}{\min}\underset{D}{\max}V(D,G) = \mathbb{E}_{x \sim p_{data}(x)}[\log D(x)] + \mathbb{E}_{z \sim p_{z}(z)}[\log (1-D(G(z)))] ~~~ (1) $$

Ta có $ D(y) $ là xác suất y nằm trong plausible data.

$$ \begin{case}