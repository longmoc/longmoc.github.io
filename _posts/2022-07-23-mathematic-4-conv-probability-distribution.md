---
layout: single
title: "Convolution of Probability Distribution"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
  teaser: /assets/images/mathematic.jpeg
---

*Cách tìm ra phân phối tổng của hai biến ngẫu nhiên độc lập sử dụng Convolution*

---

Trước tiên hãy nhắc lại Law of Total Probability (LTP) cho biến ngẫu nhiên rời rạc:

Với $X$, $Y$ là hai biến ngẫu nhiên rời rạc:

$$ p_X(x) = \sum_y p_{X,Y}(x,y) = \sum _y p_{X|Y}(x|y)p_Y(y)$$

Đối với biến liên tục, nếu $X$, $Y$ là hai biến ngẫu nhiên liên tục:

$$ f_X(x) = \int_{-\infty}^{+\infty} f_{X,Y}(x,y) \ dy = \int_{-\infty}^{+\infty} f_{X|Y}(x|y)f_Y(y) \ dy$$

## Convolution

Trong xác suất thống kê, convolution là phép toán cho phép ta lấy được phân phối tổng của hai biến rời rạc. Giả sử 
một công ty khai thác có sản lượng vàng khai thác được là $X$ tấn mỗi năm ở nước A và $Y$ tấn mỗi năm ở nước B. việc 
khai thác của công ty giữa các nước này là hoàn toàn độc lập với nhau, và bạn đã có một vài phân phối để mô hình hóa chúng. 
Vậy phân phối tổng lượng vàng khai thác được, $Z = X+Y$, là gì?

Để giải những bài toán như trên, hay tìm hiểu một ví dụ cụ thể như sau:

`Cho $X,Y \sim Unif(1,4)$`

<div align="center">.</div>
---