---
layout: posts
title: "Earth-Mover/Wasserstein distance & Kantorovich-Rubistein duality"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
---

*Ghi chú cần thiết về khoảng cách Wasserstetin và đối ngẫu Kantorovich-Rubinstein*

---

## Earth-Mover/Wasserstein distance

***Earth-Mover*** (*EM* hay *Wasserstein*) distance là một giá trị đo khoảng cách giữa hai phân phối. 
Ý tưởng của *EM* dựa trên tối thiểu hóa hoạt động của máy xúc trong việc xúc đất từ nơi này sang nơi khác với vị trí, 
hình khối định trước. Trực giác của việc cụ thể hóa ý tưởng này là coi định nghĩa phân phối xác suất qua sức nặng tại 
mỗi điểm (giống như khối lượng, thể tích đất tại mỗi $m^2$). Giả sử ta có phân phối nguồn $P$, và mong muốn di chuyển 
một số khối lượng nhất định tại những điểm trên phân phối này để biến đổi nó thành phân phối đích $Q$. Di chuyển khối 
lượng $m$ khoảng cách $d$ sẽ cần một công bằng $m \times d$. Giá trị khoảng cách *EM* có thể coi như việc tối thiểu số 
công cần thiết.

Ví dụ chúng ta có phân phối xác suất nguồn $P_r$ đại diện bởi mô đất bên trái hình dưới, phân phối xác suất đích 
$P_{\theta}$ đại diện bởi hình dạng mô đất phải. Cả hai mô đất đều có thể được lấp đầy bởi 10 khối đất. Các trạng thái 
khối đất từ $P_r$ được đặt là $x$ (1, 2, 3 như hình), từ $P_{\theta}$ được đặt là $y$ (7, 8, 9, 10).

![Ý tưởng Earth-Mover dựa trên bài toán di chuyển các khối đất]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-1.png){:style="display:block; margin-left:auto; margin-right:auto"}

Có rất nhiều cách để di chuyển các khối đất từ $P_r$ sang $P_{\theta}$, đặt các chiến lược di chuyển (*plan*) có thể 
là $\gamma$. Ví dụ về một số chiến lược di chuyển các khối đất:

![Ví dụ một vài plan]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-2.jpeg){:style="display:block; margin-left:auto; margin-right:auto"}


---

<div align="right"><i>_Hết_</i></div> 

---