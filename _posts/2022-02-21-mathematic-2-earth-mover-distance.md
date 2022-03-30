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

## Earth-Mover distance

***Earth-Mover*** (*EM*) distance là một giá trị đo khoảng cách giữa hai phân phối. 
Ý tưởng của *EM* dựa trên tối thiểu hóa hoạt động của máy xúc trong việc xúc đất từ nơi này sang nơi khác với vị trí, 
hình khối định trước. Trực giác của việc cụ thể hóa ý tưởng này là coi định nghĩa phân phối xác suất qua sức nặng tại 
mỗi điểm (giống như khối lượng, thể tích đất tại mỗi $m^2$). Giả sử ta có phân phối nguồn $P$, và mong muốn di chuyển 
một số khối lượng nhất định tại những điểm trên phân phối này để biến đổi nó thành phân phối đích $Q$. Di chuyển khối 
lượng $m$ khoảng cách $d$ sẽ cần một công bằng $m \times d$. Giá trị khoảng cách *EM* có thể coi như việc tối thiểu số 
công cần thiết.

Ví dụ chúng ta có phân phối xác suất nguồn $P_r$ đại diện bởi mô đất bên trái hình dưới, phân phối xác suất đích 
$P_{\theta}$ đại diện bởi hình dạng mô đất phải. Cả hai mô đất đều có thể được lấp đầy bởi 6 khối đất. Các trạng thái 
khối đất từ $P_r$ được đặt là $x$ (1, 2, 3 như hình), từ $P_{\theta}$ được đặt là $y$ (7, 8, 9, 10).

![Ý tưởng Earth-Mover dựa trên bài toán di chuyển các khối đất]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-1.png){:style="display:block; margin-left:auto; margin-right:auto"}

Hàm phân phối có thể viết như sau:

$$ \begin{aligned}
P_r(1) &= 3/6 = 0.5 \\
P_r(2) &= 1/6 = 0.1667 \\
P_r(3) &= 2/6 = 0.3333 \\
\\
P_{\theta}(7) &= 1/6 = 0.1667 \\
P_{\theta}(8) &= 1/6 = 0.1667 \\
P_{\theta}(9) &= 2/6 = 0.3333 \\
P_{\theta}(10) &= 2/6 = 0.3333 \\
\end{aligned} $$

Có rất nhiều cách để di chuyển các khối đất từ $P_r$ sang $P_{\theta}$, đặt các chiến lược di chuyển (*plan*) có thể 
là $\gamma$. Ví dụ về một số chiến lược di chuyển các khối đất:

![Ví dụ một vài plan]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-2.jpeg){:style="display:block; margin-left:auto; margin-right:auto"}

Cách tính công thực hiện như đã nói bằng $m \times d$, với plan ${\gamma}_1$, từ bảng phân bổ tương ứng ta tính được công này bằng:

$$ 1 \times (7-1) + 2 \times (10-1) + 1 \times (8-2) + 2 \times (9-3) = 42 $$

Không phải tất cả các plan đều cho kết quả chi phí giống nhau, trong những trường hợp vị trí, hình dạng mô đất phức 
tạp các cách di chuyển khác nhau có thể tốn chi phí khác nhau. Earth-Mover distance tương ứng với plan có chi phí nhỏ 
nhất.

Ta luôn có $\sum_x \gamma(x,y) = P_{\theta}(y)$ và $\sum_y \gamma(x,y) = P_r(x)$. Điều này là tất yếu nếu vì các cách 
di chuyển là đúng và khối đất được lấy từ phân phối nguồn $P_r$ đặt vào phân phối đích $P_{\theta}$.

![Thể hiện của joined probability distribution]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-3.jpeg){:style="display:block; margin-left:auto; margin-right:auto"}

Tương tự với $P_r$ và $P_{\theta}$ là các phân phối xác suất liên tục, tính chất này vẫn được đảm bảo:

$$ \begin{aligned} 
&\int_{x}{\gamma(x,y) \ dx} = P_{\theta}(y) \\
&\int_{y}{\gamma(x,y) \ dy} = P_r(x)
\end{aligned} $$

---

<div align="right"><i>_Hết_</i></div> 

---