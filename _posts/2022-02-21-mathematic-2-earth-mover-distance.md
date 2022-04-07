---
layout: posts
title: "Earth-Mover/Wasserstein distance & Kantorovich-Rubinstein duality"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
---

*Ghi chú cần thiết về khoảng cách Wasserstein và đối ngẫu Kantorovich-Rubinstein*

---

# Earth-Mover/Wasserstein distance

***Earth-Mover*** (*EM* hay ***Wasserstein***) *distance* là một giá trị đo khoảng cách giữa hai phân phối. 
Ý tưởng của *EM* dựa trên tối thiểu hóa hoạt động của máy xúc trong việc xúc đất từ nơi này sang nơi khác với vị trí, 
hình khối định trước. Trực giác của việc cụ thể hóa ý tưởng này là coi định nghĩa phân phối xác suất qua sức nặng tại 
mỗi điểm (giống như khối lượng, thể tích đất tại mỗi $m^2$). Giả sử ta có phân phối nguồn $P$, và mong muốn di chuyển 
một số khối lượng nhất định tại những điểm trên phân phối này để biến đổi nó thành phân phối đích $Q$. Di chuyển khối 
lượng $m$ khoảng cách $d$ sẽ cần một công bằng $m \times d$. Giá trị khoảng cách *EM* có thể coi như việc tối thiểu số 
công cần thiết.

Ví dụ chúng ta có phân phối xác suất nguồn $P_r$ đại diện bởi mô đất bên trái hình dưới, phân phối xác suất đích 
$P_{\theta}$ đại diện bởi hình dạng mô đất phải. Cả hai mô đất đều có thể được lấp đầy bởi 13 khối đất và được chia 
thành 5 cột đất. Các cột đất của $P_r$ (tương ứng các trạng thái của phân phối) được đặt là $x$, của $P_{\theta}$ được 
đặt là $y$.

![Ý tưởng Earth-Mover dựa trên bài toán di chuyển các khối đất]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-1.png){:style="display:block; margin-left:auto; margin-right:auto"}

Hàm phân phối có thể viết như sau:

$$ \begin{aligned}
P_r(1) &= 3/13 \, , P_r(2) &= 1/13 \\
P_r(3) &= 2/13 \, , P_r(4) &= 2/13 \\
P_r(5) &= 5/13 \\
\\
P_{\theta}(1) &= 2/13 \, , P_{\theta}(2) &= 1/13 \\
P_{\theta}(3) &= 3/13 \, , P_{\theta}(4) &= 4/13 \\
P_{\theta}(5) &= 3/13 \\
\end{aligned} $$

Yêu cầu bài toàn là di chuyển các khối đất của $P_r$ để thành $P_{\theta}$, ta viết được khoảng cách khi di chuyển đất 
giữa các cột đất (hay khoảng cách giữa các state trong phân phối) như sau:  

![Khoảng cách giữa các cột đất]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-2.svg){:style="display:block; margin-left:auto; margin-right:auto"}

Có rất nhiều cách để di chuyển các khối đất từ $P_r$ thành $P_{\theta}$, đặt các chiến lược di chuyển (*plan*) có thể 
là $\gamma \in \Pi$. Ví dụ về một số chiến lược di chuyển các khối đất:

![Ví dụ một vài plan]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-3.svg){:style="display:block; margin-left:auto; margin-right:auto"}

Cách tính công thực hiện như đã nói bằng $m \times d$, với plan ${\gamma}_1$, từ bảng phân bổ của plan ta tính được 
công tương ứng:

$$ \begin{aligned}
\sum_{x,y}\gamma(x,y) D(x,y) &=  &\gamma_1(1,1) D(1,1) + \gamma_1(1,2) D(1,2) + \gamma_1(1,3) D(1,3) + ... \\
                             &\text{ } +&\gamma_1(2,1) D(2,1) + \gamma_1(2,2) D(2,2) + \gamma_1(2,3) D(2,3) + ... \\
                             &\text{ } +&... \\
                             &=  &2 \times 0 + 0 \times 1 + 1 \times 2 + ... \\
                             &\text{ } +&0 \times 1 + 1 \times 0 + 0 \times 1 + ... \\
                             &\text{ } +&... \\
                             &= &4
\end{aligned} $$

Không phải tất cả các plan đều cho kết quả chi phí giống nhau, trong những trường hợp vị trí, hình dạng mô đất phức 
tạp các cách di chuyển khác nhau có thể tốn chi phí khác nhau. Ví dụ với plan ${\gamma}_2$ công tính được là 6. 
*Earth-Mover distance* tương ứng với plan có chi phí nhỏ 
nhất.

Ta luôn có $\sum_x \gamma(x,y) = P_{\theta}(y)$ và $\sum_y \gamma(x,y) = P_r(x)$. Điều này là tất yếu nếu vì các cách 
di chuyển là đúng và khối đất được lấy từ phân phối nguồn $P_r$ đặt vào phân phối đích $P_{\theta}$.

![Thể hiện của joined probability distribution]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-earthmover-4.svg){:style="display:block; margin-left:auto; margin-right:auto"}

Tương tự với $P_r$ và $P_{\theta}$ là các phân phối xác suất liên tục, tính chất này vẫn được đảm bảo:

$$ \begin{aligned} 
&\int_{x}{\gamma(x,y) \ dx} = P_{\theta}(y) \\
&\int_{y}{\gamma(x,y) \ dy} = P_r(x)
\end{aligned} $$

Có thể thấy $\gamma \in \Pi(P_r, P_{\theta}$ là *phân phối xác suất đồng thời* (*joint probability distribution*) của 
$P_r$, $P_{\theta}$ và $P_r$, $P_{\theta}$ là các *phân phối biên* (*margin distribution*) của $\gamma \in \Pi$.

Xét khoảng cách giữa các trạng thái của hai phân phối là $$\|x - y\|$$. Chi phí di chuyển hay khoảng cách giữa phép dịch 
chuyển phân phối $p_r$ sang $P_{\theta}$ được viết qua công thức:

$$\sum_x \sum_y \gamma(x,y) \| x - y \| = \mathbb{E}_{(x,y) \sim \gamma}\big[\|x - y\|\big]$$

Hoặc đối với trường hợp phân phối liên tục:

$$\int_x \int_y \gamma(x,y) \| x - y \| \,dy\,dx = \mathbb{E}_{(x,y) \sim \gamma}\big[\|x - y\|\big]$$

*Earth-Mover distance* $EMD(P_r, P_{\theta})$ (đối với trường hợp phân phối rời rạc) hoặc *Wasserstein distance* 
$W(P_r, P_{\theta})$ (đối với trường hợp phân phối liên tục) là giá trị ***infimum*** hay cận dưới nhỏ nhất của khoảng cách trên:

$$ W(P_r, P_{\theta}) = \inf_{\gamma \in \Pi(P_r ,P_{\theta})} \mathbb{E}_{(x,y) \sim \gamma}\big[\|x - y\|\big] $$

# Kantorovich-Rubinstein duality

---

<div align="right"><i>_Hết_</i></div> 

---