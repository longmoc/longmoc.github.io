---
layout: posts
title: "Cross-entropy, Kullback-Leibler divergence & Jensen-Shannon divergence"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
---

*Các khái niệm được sử dụng trong Machine learning như Cross-entropy, Kullback-Leibler divergence và 
Jensen-Shannon divergence được bắt nguồn từ lý thuyết thông tin.*

---

## Entropy

Cơ sở của entropy dựa trên ý tưởng định lượng có bao nhiêu thông tin trong một bản tin. Nói một cách tổng quát hơn: 
định lượng lượng thông tin của sự kiện (event) hay biến ngẫu nhiên (random variable) gọi là entropy. 
Entropy được tính dựa trên xác suất sự kiện.

### Tính toán thông tin của event

Là việc đo lường độ bất ngờ, không chắc chắn của event.

- Low probability event: High information $ \sim $ surprising
- Hight probability event: Low information $ \sim $ unsurprising

(Nói thêm về ý nghĩa trực giác: một event không chắc chắn (unlikely event) được xảy ra sẽ cho nhiều thông tin hơn 
việc biết một sự kiện chắc chắn (likely event) được xảy ra).

Ta có thể tính toán lượng thông tin của event bằng cách sử dụng xác suất của event đó:

$$ \mathrm{Information}(x) = -\log (p(x)) $$ 

Trong lý thuyết thông tin người ta sử dụng base-2 logarithm, lượng thông tin đo bằng binary, tượng trưng cho số bit 
cần thiết để mã hóa thông tin.

Tính toán thông tin được ký hiệu là $ h() $: $ h(x) = -log(p(x))

### Entropy của biến ngẫu nhiên

Biến ngẫu nhiên $ X$ với phân phối 



