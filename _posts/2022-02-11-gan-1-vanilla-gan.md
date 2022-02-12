---
layout: posts
title: "Generative Adversarial Networks I: Vanilla GAN"
categories: generative-adversarial-networks
mathjax: true
header:
  image: /assets/images/gan.jpeg
  image_description: "Creative"
---

Note mở đầu trong chuỗi notepage về Generative Adversarial Network (GAN) và các biến thể sẽ đề cập đến mô hình gốc, khởi nguyên của GAN.

### Generative Adversarial Network

Mô hình sinh, bài toán unsupervised

Giải quyết unsupervised bằng supervised

Generator

Discriminator

### Loss function

Hàm loss của GAN có mục đích kết hợp tối ưu mục tiêu của cả Discriminator và Generator
$$ \min\underset{G}\max\underset{D}(D,G) = \mathbb{E}_{x \sim p_{data}(x)}[\log D(x)] + \mathbb{E}_{z \sim p_{z}(z)}[\log (1-D(G(z)))]$$