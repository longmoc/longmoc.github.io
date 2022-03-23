---
layout: posts
title: "Earth-Mover/Wasserstein distance & Kantorovich-Rubistein duality"
categories: mathematic
mathjax: true
header:nn
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

Rộng hơn, có thể liên hệ khoảng các của hai phân phối qua việc dịch chuyển các khối lượng tại các điểm của hai 
phân phối đó để chúng trở nên giống nhau. Ví dụ ta có hai phân phối $P$ và $Q$, mỗi phân phối đều có 13 khối lượng 
đất và trải trên 5 ô. Phân bố khối lượng tại các ô (hay sức nặng tại các điểm như đã nói ở trên) cụ thể như sau:

$$ P(p_1=3, p_2=1, p_3=2, p_4=2, p_5=5) $$
$$ Q(q_1=2, q_2=1, q_3=3, q_4=4, q_5=3) $$

Phương án biến đổi $P$ và $Q$ để chúng trở nên giống nhau:
- Step 1: Di chuyển 1 khối từ $p_1$ sang $p_2$, $p_1$ và $q_1$ giống nhau
- Step 2: Di chuyển 1 khối từ $p_2$ sang $p_3$, $p_2$ và $q_2$ giống nhau
- Step 3: Giữ nguyên $p_3$ do $p_3$ và $q_3$ đã giống nhau.
- Step 4: Di chuyển 1 khối từ $p_1$ sang $p_2$, $p_1$ và $q_1$ đã giống với nhau
- Step 5: Giữ nguyên $p_3$ do $p_3$ và $q_3$ đã giống nhau.

![Phương án di chuyển các khối của 2 phân phối để chúng giống nhau]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-example.png){:style="display:block; margin-left:auto; margin-right:auto"}


---

<div align="right"><i>_Hết_</i></div> 

---