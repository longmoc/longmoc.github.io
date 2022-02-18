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

Tính toán thông tin được ký hiệu là $ h() $: $ \ h(x) = -log(p(x)) $

### Entropy của biến ngẫu nhiên

Biến ngẫu nhiên *rời rạc* $ X$ với phân phối xác suất $ P $, $ H(X) $ gọi là *information entropy* hay *Shannon entropy* 
hay đơn giản hơn là *entropy*.

Trực giác: số bit trung bình cần thiết để biểu diễn hoặc truyền một event được lấy từ phân phối xác suất của biến 
ngẫu nhiên.

Ví dụ tính *entropy* của biến ngẫu nhiên $ X $ với $k \in K$ trạng thái rời rạc:

$$ H(X) = - \sum_{k\in K} p(k)\cdot \log (p(k)) $$

Cũng giống như *information* $ h() $, hàm $\log ()$ được sử dụng là hàm base-2 với đơn vị *bits*. Hoặc có thể dùng 
logarithm tự nhiên (natural logarithm, base-e logarithm) với đơn vị tính được là *nats*.

*Entropy* của biến ngẫu nhiên rời rạc nhỏ nhất khi phân phối chỉ có một event với xác suất bằng 1 và lớn nhất khi 
mọi event đều có khả năng xảy ra như nhau.

Trong trường hợp biến ngẫu nhiên liên tục, Claude Shannon - cha đẻ của lý thuyết thông tin cũng như entropy - 
đã đưa ra công thức của entropy cho phân phối liên tục (continuous distribution), hay còn gọi là *differential entropy*:

$$ h(X) = - \int{p(x)\log p(x) \ dx} $$

Tuy nhiên công thức này không phải kết quả của một đạo hàm nào và không có đẩy đủ các tính chất như entropy rời rạc để 
trở nên có ý nghĩa trong việc đo lường tính bất ngờ của event.

## Cross-entropy

Dựa trên ý tưởng entropy, cross-entropy tính toán số bit cần thiết để biểu diễn hoặc truyền một event từ một 
phân phối này sang phân phối khác. Một cách diễn đạt khác trong Machine learning thì *cross-entropy là trung bình 
số bits cần thiết để mã hóa dữ liệu từ nguồn với phân phối $p$ khi chúng ta sử dụng model $q$*. Trực giác của khái niệm 
này đến từ việc giả sử ta có một phân phối đích (một phân phối xác suất nền tảng) $P$ và $Q$ là một *xấp xỉ* của phân phối 
đích, khi đó cross-entropy của $Q$ xét từ $P$ là số bits cần thêm để biểu diễn một sự kiện nếu dùng $Q$ thay vì $P$.

Cross-entropy giữa hai phân phối xác suất $Q$ và $P$ được viết dưới dạng:

$$ H(P,Q) $$

Trong đó $H()$ là hàm cross-entropy, $P$ là phân phối đích, $Q$ là xấp xỉ của phân phối đích.

Công thức tính cross-entropy sử dụng xác suất event của $P$ và $Q$ trên biến rời rạc như sau:

$$ H(P,Q) = -\sum_{x}P(x) \cdot \log (Q(x)) $$

Với $P(x)$ là xác suất xảy ra event $x$ trên $P$, $Q(x)$ là xác suất xảy ra event $x$ trên $Q$.

Công thức tương tự có thể dùng cho phân phối xác suất liên tục:

$$ H(P,Q) = -\int_{x}P(x)\log (Q(x)) \ dx$$

Giá trị cross-entropy là một số dương đơn vị bits và sẽ bằng entropy của phân phối nếu hai phân phối xác suất là giống 
hệt nhau. 

Có thể biểu diễn cross-entropy dưới dạng *giá trị kỳ vọng* (expected value) trên phân phối $P$ với function $\log (Q)$:

$$ H(P,Q) = -\mathbb{E}_P[\log(Q)] $$

*Ký hiệu của cross-entropy* $ H(P,Q) $ *cũng được dùng cho [**joint entropy**](https://en.wikipedia.org/wiki/Joint_entropy) 
nên có thể xảy ra nhầm lẫn.*

## Kullback-Leibler divergence

Việc so sánh các phân phối xác suất thường được đưa về bài toán tính toán *khoảng cách thống kê (statistical distance)* 
giữa hai phân phối. Một cách tiếp cận cho vấn đề này là tính *distance measure*. Điều này thực tế rất khó làm được, 
thay vào đó, cách phổ biến hơn là tính độ *phân kỳ (divergence)* giữa hai phân phối xác suất. *Divergence* cũng giống như 
*measure* nhưng không có tính đối xứng. Có nghĩa là *divergence* chỉ thể hiện độ khác của một phân phối so với phân phối 
khác, do đó *divergence* của phân phối $P$ và $Q$ sẽ khác *divergence* của phân phối $Q$ và $P$.

***Kullback-Leibler divergence*** score hay *KL divergence* score định lượng một phân phối xác suất khác biệt như thế 
nào so với một phân phối xác suất khác.

*KL divergence* giữa hai phân phối $Q$ và $P$ thường được viết dưới dạng $$ KL(P \| Q) $$. Với $$\|$$ là toán tử thể hiện 
sự *phân kỳ* hay $P$ phân kỳ từ $Q$. Công thức tính KL divergence score của phân phối rời rạc:

$$ KL(P\|Q) = -\sum_{x}{P(x)\log \biggl(\frac{Q(x)}{P(x)}\biggr)} $$

Trực giác ý nghĩa của KL divergence score là khi xác suất xảy ra event từ $P$ lớn nhưng xác suất xảy ra event đó từ $Q$ 
lại nhỏ thì độ phân kỳ lớn. Nếu xác suất xảy ra event từ $P$ nhỏ nhưng từ $Q$ lớn thì độ phân kỳ cũng lớn, tuy nhiên 
trường hợp này divergence score không đạt được độ lớn tương đương với trường hợp trước.

Đối với phân phối xác suất của biến liên tiếp, công thức tính có dạng:

$$ KL(P\|Q) = -\int_{x}{P(x)\log \biggl(\frac{Q(x)}{P(x)}\biggr) \ \mathrm{d}x} $$




