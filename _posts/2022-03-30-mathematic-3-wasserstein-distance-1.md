---
layout: posts
title: "Wasserstein distance I: Earth-Mover distance"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
---

*Ghi chú cần thiết về khoảng cách Wasserstein. Do muốn viết lại đầy đủ những điều học được nên mình viết thành hai 
phần. Phần một này là ghi chép của mình về khoảng cách Earth-Mover/Wasserstein và ý nghĩa của nó*

---

# Earth-Mover/Wasserstein distance

***Earth-Mover*** (*EM* hay ***Wasserstein***) *distance* là một giá trị đo khoảng cách giữa hai phân phối. 
Ý tưởng của *EM* dựa trên tối thiểu hóa hoạt động của máy xúc trong việc xúc đất từ nơi này sang nơi khác với vị trí, 
hình khối định trước. Trực giác của việc cụ thể hóa ý tưởng này là coi định nghĩa phân phối xác suất qua sức nặng tại 
mỗi điểm (giống như khối lượng, thể tích đất tại mỗi $m^2$). Giả sử ta có phân phối nguồn $P$, và mong muốn di chuyển 
một số khối lượng nhất định tại những điểm trên phân phối này để biến đổi nó thành phân phối đích $Q$. Di chuyển khối 
lượng $m$ khoảng cách $d$ sẽ cần một công bằng $m \times d$. Giá trị khoảng cách *EM* có thể coi như việc tối thiểu số 
công cần thiết.

Ví dụ chúng ta có phân phối xác suất nguồn $P_{\theta}$ đại diện bởi mô đất bên trái hình dưới, phân phối xác suất đích 
$P_r$ đại diện bởi hình dạng mô đất phải. Cả hai mô đất đều có thể được lấp đầy bởi 13 khối đất và được chia 
thành 5 cột đất. Các cột đất của $P_{\theta}$ (tương ứng các trạng thái của phân phối) được đặt là $x$, của $P_r$ được 
đặt là $y$. Yêu cầu bài toàn là di chuyển các khối đất của $P_{\theta}$ để thành $P_r$.

![Ý tưởng Earth-Mover dựa trên bài toán di chuyển các khối đất]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-wasserstein-distance-1.svg){:style="display:block; margin-left:auto; margin-right:auto"}

Ta viết được hàm phân phối cũng như bảng khoảng cách $D$ khi di chuyển đất 
giữa các cột đất (hay khoảng cách giữa các state trong phân phối):  

$$ \begin{aligned}
P_{\theta}(1) &= 3/13 \, , &P_{\theta}(2) = 1/13 \\
P_{\theta}(3) &= 2/13 \, , &P_{\theta}(4) = 2/13 \\
P_{\theta}(5) &= 5/13 \\
\\
P_r(1) &= 2/13 \, , &P_r(2) = 1/13 \\
P_r(3) &= 3/13 \, , &P_r(4) = 4/13 \\
P_r(5) &= 3/13 \\
\end{aligned} $$

![Khoảng cách giữa các cột đất]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-wasserstein-distance-2.svg){:style="display:block; margin-left:auto; margin-right:auto"}

Có rất nhiều cách để di chuyển các khối đất từ $P_{\theta}$ thành $P_r$, đặt các chiến lược di chuyển (*plan*) có thể 
là $\gamma \in \Pi$. Ví dụ về một số chiến lược di chuyển các khối đất:

![Ví dụ một vài plan]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-wasserstein-distance-3.svg){:style="display:block; margin-left:auto; margin-right:auto"}

(Để chính xác hơn thì con số trong bảng $\gamma$ phải chia cho 13 - số nguyên được để vì mục đích dễ nhìn, và có thể 
cũng dễ hiểu hơn)

Cách tính công thực hiện như đã nói bằng $m \times d$. Ví dụ với plan ${\gamma}_1$, từ bảng phân bổ của plan ta tính được 
công tương ứng:

$$ \begin{aligned}
&&&\sum_{x,y}\gamma_1(x,y) D(x,y) \\
&=&&\gamma_1(1,1) D(1,1) + \gamma_1(1,2) D(1,2) + \gamma_1(1,3) D(1,3) + \ ... \\
&&+\ &\gamma_1(2,1) D(2,1) + \gamma_1(2,2) D(2,2) + \gamma_1(2,3) D(2,3) +  \ ... \\
&&+\ &... \\
&=&&\frac{2}{13} \times 0 + 0 \times 1 + \frac{1}{13} \times 2 + \ ... \\
&&+\ &0 \times 1 + \frac{2}{13} \times 0 + 0 \times 1 + \ ... \\
&&+\ &... \\
&=&&\frac{4}{13}
\end{aligned} $$

Không phải tất cả các plan đều cho kết quả chi phí giống nhau, trong những trường hợp vị trí, hình dạng mô đất phức 
tạp các cách di chuyển khác nhau có thể tốn chi phí khác nhau. Ví dụ với plan ${\gamma}_2$ công tính được là $\frac{6}{13}$. 
*Earth-Mover distance* tương ứng với công của plan có chi phí nhỏ 
nhất.

Ta luôn có $\sum_x \gamma(x,y) = P_r(y)$ và $\sum_y \gamma(x,y) = P_{\theta}(x)$. Điều này là tất yếu vì các cách 
di chuyển là đúng và khối đất được lấy từ phân phối nguồn $P_{\theta}$ đặt vào phân phối đích $P_r$.

![Thể hiện của joined probability distribution]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-wasserstein-distance-4.svg){:style="display:block; margin-left:auto; margin-right:auto"}

Tương tự với $P_{\theta}$ và $P_r$ là các phân phối xác suất liên tục, tính chất này vẫn được đảm bảo:

$$ \begin{aligned} 
&\int_{x}{\gamma(x,y) \ dx} = P_r(y) \\
&\int_{y}{\gamma(x,y) \ dy} = P_{\theta}(x)
\end{aligned} $$

Có thể thấy $\gamma \in \Pi(P_{\theta},P_r)$ là *phân phối xác suất đồng thời* (*joint probability distribution*) của 
$P_{\theta}$, $P_r$ và $P_{\theta}$, $P_r$ là các *phân phối biên* (*margin distribution*) của $\gamma \in \Pi$.

Xét khoảng cách giữa các trạng thái của hai phân phối là $$\|x - y\|$$. Chi phí di chuyển hay khoảng cách giữa phép dịch 
chuyển phân phối $P_{\theta}$ sang $P_r$  được viết qua công thức:

$$\sum_x \sum_y \gamma(x,y) \| x - y \| = \mathbb{E}_{(x,y) \sim \gamma}\big[\|x - y\|\big]$$

Hoặc đối với trường hợp phân phối liên tục:

$$\int_x \int_y \gamma(x,y) \| x - y \| \,dy\,dx = \mathbb{E}_{(x,y) \sim \gamma}\big[\|x - y\|\big]$$

*Earth-Mover distance* $\mathrm{EMD}(P_{\theta}, P_r)$ (đối với trường hợp phân phối rời rạc) hoặc *Wasserstein distance* 
$W(P_{\theta}, P_r)$ (đối với trường hợp phân phối liên tục) là giá trị ***infimum*** hay cận dưới nhỏ nhất của khoảng 
cách trên:

$$ \mathrm{EMD}(P_{\theta}, P_r) = \inf_{\gamma \in \Pi(P_{\theta}, P_r)} \mathbb{E}_{(x,y) \sim \gamma}\big[\|x - y\|\big] $$

$$ W(P_{\theta}, P_r) = \inf_{\gamma \in \Pi(P_{\theta}, P_r)} \mathbb{E}_{(x,y) \sim \gamma}\big[\|x - y\|\big] $$

Công thức này khi đưa về dạng đối ngẫu của chúng (mà ta sẽ phân tích ở phần sau):

$$ W(P_\theta, P_r) = \sup_{\lVert f \lVert_{L \leq 1}} \ \mathbb{E}_{x \sim P_{\theta}}[f(x)] - \mathbb{E}_{x \sim P_r}[f(x)] $$

# Why Wasserstein Distance

Để hiểu tại sao chúng ta cần Wasserstein distance và khác biệt so với những KL/JS divergence, hãy tìm hiểu ví dụ:

Xét các phân phối xác suất xác định trên $\mathbb{R}^2$, phân phối ảnh dữ liệu thực tế $P_r=(0, x)$ với $x$ được lấy mẫu 
ngẫu nhiên đồng nhất từ $U[0, 1]$. Xét các họ phân phối sinh $P_{\theta}$ dạng $P_{\theta}=(0, x)$, $x$ cũng từ $U[0, 1]$.

Biểu đồ minh họa phân phối thực và sinh với $\theta = 1$:

![Phân phối thực và sinh khi theta = 1]({{ site.url }}{{ site.baseurl }}/assets/images/posts/m2-wasserstein-distance-5.png){:style="display:block; margin-left:auto; margin-right:auto"}

Thuật toán tối ưu sẽ cần được huấn luyện để di chuyển $\theta$ về $0$. Khi tiến dần $\theta \to 0$ khoảng cách 
$d(P_r, P_\theta)$ sẽ giảm dần. Nhưng không may điều này không phải lúc nào cũng xảy ra.

- Với các đánh giá dạng discriminative, ta luôn có:

$$\delta(P_r, P_\theta) =
  \begin{cases}
    1 &\quad \text{if } \theta \neq 0~, \\
    0 &\quad \text{if } \theta = 0~.
  \end{cases}
$$

- KL divergence: $KL(P\|Q)$ có giá trị bằng $+\infty$ nếu tồn tại điểm $(x, y)$ tại đó $P(x,y) > 0$ và $Q(x,y)=0) (do
  $$\lim_{x \to 0} \log (x) = -\infty$$). $KL(P_{\theta}\|P_0)=+\infty$ tại $(\theta, y)$ với $y \in [0,1]$. 
  Ngược lại các điểm $(0, y) ta có $KL(P_0\|P_{\theta})=+\infty$
  
---

<div align="right"><i>Còn tiếp</i></div> 

---