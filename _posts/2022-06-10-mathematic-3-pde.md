---
layout: single
title: "Partial Differential Equations"
categories: mathematic
mathjax: true
header:
  image: /assets/images/mathematic.jpeg
  image_description: "Mathematic"
  teaser: /assets/images/mathematic.jpeg
---

*Phương trình vi phân thông thường chỉ mô tả các hệ động học một chiều, trong khi phần lớn các hệ động học trong tự nhiên 
(chuyển động của âm thanh, nhiệt, sự phân tán tĩnh điện học...) lại là những hệ đa chiều, biến đổi theo cả không gian và 
thời gian. Các hệ như vậy được mô tả thông qua **Phương trình vi phân từng phần (Partial Differential Equation - PDE)**. 
PDE vì thế trở thành một công cụ toán học quan trọng để liên hệ và tổng quát hóa nhiều hiện tượng tự nhiên.*
{: .text-justify}

---

## Phương trình vi phân từng phần (Partial Differential Equation)

Một PDE là một phương trình toán học liên quan đến hai hoặc nhiều biến độc lập, một hàm đa biến chưa biết (phụ thuộc vào 
các biến đó) và các đạo hàm riêng của hàm theo các biến độc lập tương ứng. Bậc của PDE là bậc cao nhất của đạo hàm xuất 
hiện trong phương trình. Một *nghiệm* (hay *nghiệm riêng* - *particular solution*) của PDE là một *hàm* sao cho khi thay 
vào phương trình, đẳng thức được thỏa mãn. Một nghiệm được gọi là *nghiệm chung* (*general solution*) nếu nó chứa tất cả 
các nghiệm riêng của phương trình.
{: .text-justify}

Một nghiệm đúng (*exact solution*) là một nghiệm riêng thu được dưới dạng tường minh cho phương trình vi phân từng phần, 
đặc biệt đáng quan tâm với các phương trình phi tuyến hoặc bậc cao.
{: .text-justify}

## Phương trình vi phân từng phần bậc nhất (First-Order Partial Differential Equation)

### General Form

Một *phương trình vi phân từng phần bậc nhất* (*first-order PDE*) với $n$ biến độc lập có dạng chuẩn:
{: .text-justify}

$$F\biggl(x_1,x_2,\dots, x_n,w,\frac{\partial w}{\partial x_1},
\frac{\partial w}{\partial x_2},\dots,\frac{\partial w}{\partial x_n}\biggr)=0$$

với $w=w(x_1,x_2,\dots, x_n)$ là hàm đa biến chưa biết và $F(\dots)$ là hàm số đã cho.
{: .text-justify}

### Quasilinear Equations

Một *phương trình vi phân từng phần tựa tuyến tính bậc nhất* (*first-order quasi-linear PDE*) với hai biến độc lập có dạng:
{: .text-justify}

$$ f(x,y,w)\frac{\partial w}{\partial x}+g(x,y,w)\frac{\partial w}{\partial y}=h(x,y,w). \tag{1}\label{1}$$

Các phương trình dạng này xuất hiện trong nhiều lĩnh vực như cơ học liên tục, khí động học, thủy động lực học, dẫn truyền 
nhiệt, lý thuyết sóng, âm học, dòng chảy nhiều pha, kỹ thuật hóa học...
{: .text-justify}

Nếu các hàm $f$, $g$ và $h$ độc lập so với $w$ chưa biết thì phương trình $(\ref{1})$ là *tuyến tính* (*linear*).
{: .text-justify}

Đối với lớp phương trình này, phương pháp quan trọng nhất là *phương pháp đặc trưng* (*method of characteristics*). 
Ý tưởng là tìm những đường cong đặc biệt mà dọc theo đó PDE được đưa về một hệ phương trình vi phân thường đơn giản hơn.
{: .text-justify}

#### Hệ đặc trưng (Characteristic system) và Nghiệm tổng quát (General solution)

Hệ các phương trình vi phân thường
{: .text-justify}

$$\frac{dx}{f(x,y,w)}=\frac{dy}{g(x,y,w)}=\frac{dw}{h(x,y,w)} \tag{2}\label{2}$$

là *hệ phương trình đặc trưng* (*characteristic system*) của phương trình $(\ref{1})$. Giả sử tìm được hai tích phân độc lập 
của hệ này dưới dạng:
{: .text-justify}

$$\tag{3}\label{3}
u_{1}(x, y, w)=C_{1},\qquad u_{2}(x,y,w)=C_{2},$$

với $C_1$ và $C_2$ là các hằng số tùy ý. Khi đó $u_1$ và $u_2$ là hai đại lượng bất biến dọc theo các đường đặc trưng, và 
nghiệm chung của phương trình $(\ref{1})$ có thể viết dưới dạng:
{: .text-justify}

$$\tag{4}\label{4}
\Phi(u_{1},u_{2})=0$$

trong đó $\Phi$ là một hàm bất kỳ của hai biến.
{: .text-justify}

### Cách sử dụng hệ đặc trưng (Characteristic system)

Ý tưởng của phương pháp đặc trưng là đưa PDE bậc nhất về một hệ ODE dọc theo các đường cong đặc biệt trong không gian 
$(x,y,w)$. Trên mỗi đường cong đặc trưng đó, vi phân của $w$ thỏa:
{: .text-justify}

$$
\frac{dw}{ds}=h(x,y,w), \qquad \frac{dx}{ds}=f(x,y,w), \qquad \frac{dy}{ds}=g(x,y,w).
$$

Nếu tìm được hai tích phân độc lập của hệ đặc trưng $(\ref{2})$ thì ta sẽ thu được hai đại lượng bất biến dọc theo các 
đường đặc trưng. Từ đó, nghiệm của PDE được biểu diễn dưới dạng một quan hệ ẩn giữa hai bất biến này như trong $(\ref{4})$.
{: .text-justify}

Trong thực hành, các bước giải thường là:
{: .text-justify}

* Viết hệ đặc trưng tương ứng với PDE.
  {: .text-justify}
* Tìm hai tích phân độc lập của hệ đó bằng cách giải ODE hoặc so sánh các tỉ số.
  {: .text-justify}
* Ghép hai tích phân vào dạng $\Phi(u_1,u_2)=0$ để thu nghiệm tổng quát.
{: .text-justify}

## Ví dụ giải phương trình bậc nhất

### Ví dụ 1

Xét phương trình
{: .text-justify}

$$
\frac{\partial w}{\partial x}+\frac{\partial w}{\partial y}=0. \tag{5}\label{5}
$$

Ta có $f=1$, $g=1$, $h=0$, do đó hệ đặc trưng là:
{: .text-justify}

$$
\frac{dx}{1}=\frac{dy}{1}=\frac{dw}{0}. \tag{6}\label{6}
$$

Từ $\frac{dx}{1}=\frac{dy}{1}$ suy ra:
{: .text-justify}

$$
dy-dx=0 \Rightarrow y-x=C_1.
$$

Từ $\frac{dw}{0}=\frac{dx}{1}$ suy ra $dw=0$, nghĩa là:
{: .text-justify}

$$
w=C_2.
$$

Do đó hai tích phân độc lập của hệ đặc trưng là:
{: .text-justify}

$$
u_1=y-x,\qquad u_2=w.
$$

Theo $(\ref{4})$, nghiệm tổng quát của $(\ref{5})$ có dạng:
{: .text-justify}

$$
\Phi(y-x,w)=0
$$

hay tương đương:
{: .text-justify}

$$
w=F(y-x),
$$

trong đó $F$ là một hàm khả vi tùy ý.
{: .text-justify}

### Ví dụ 2

Xét phương trình tựa tuyến tính:
{: .text-justify}

$$
x\frac{\partial w}{\partial x}+y\frac{\partial w}{\partial y}=w. \tag{7}\label{7}
$$

Hệ đặc trưng tương ứng là:
{: .text-justify}

$$
\frac{dx}{x}=\frac{dy}{y}=\frac{dw}{w}. \tag{8}\label{8}
$$

Từ $\frac{dx}{x}=\frac{dy}{y}$ suy ra:
{: .text-justify}

$$
\ln|x|-\ln|y|=C_1
\Rightarrow
\frac{x}{y}=C_1.
$$

Từ $\frac{dx}{x}=\frac{dw}{w}$ suy ra:
{: .text-justify}

$$
\ln|x|-\ln|w|=C_2
\Rightarrow
\frac{w}{x}=C_2.
$$

Vì vậy có thể chọn:
{: .text-justify}

$$
u_1=\frac{x}{y},\qquad u_2=\frac{w}{x}.
$$

Suy ra nghiệm tổng quát của $(\ref{7})$ là:
{: .text-justify}

$$
\Phi\left(\frac{x}{y},\frac{w}{x}\right)=0
$$

hay viết tường minh hơn:
{: .text-justify}

$$
w=xF\left(\frac{x}{y}\right).
$$

## Bài toán Cauchy cho PDE bậc nhất

Trong nhiều ứng dụng, ngoài phương trình PDE người ta còn cho thêm dữ kiện ban đầu trên một đường cong trong mặt phẳng 
$(x,y)$. Khi đó ta thu được *bài toán Cauchy*.
{: .text-justify}

Giả sử cần giải phương trình $(\ref{1})$ kèm điều kiện:
{: .text-justify}

$$
x=x_0(\tau),\qquad y=y_0(\tau),\qquad w=w_0(\tau). \tag{9}\label{9}
$$

Nói cách khác, tại thời điểm ban đầu $s=0$, nghiệm được biết trên một đường cong tham số hóa bởi $\tau$. Từ mỗi điểm trên 
đường cong ban đầu, hệ đặc trưng sinh ra một đường cong trong không gian $(x,y,w)$. Nghiệm của PDE được thu bằng cách lan 
truyền dữ kiện ban đầu dọc theo các đường đặc trưng.
{: .text-justify}

Về nguyên tắc, ta giải hệ:
{: .text-justify}

$$
\frac{dx}{ds}=f(x,y,w),\qquad
\frac{dy}{ds}=g(x,y,w),\qquad
\frac{dw}{ds}=h(x,y,w)
$$

với điều kiện đầu:
{: .text-justify}

$$
x(0,\tau)=x_0(\tau),\qquad
y(0,\tau)=y_0(\tau),\qquad
w(0,\tau)=w_0(\tau).
$$

Nếu có thể giải được và phép đổi biến từ $(s,\tau)$ sang $(x,y)$ là khả nghịch cục bộ thì ta sẽ biểu diễn được $w$ như một 
hàm của $x$ và $y$.
{: .text-justify}

### Điều kiện không đặc trưng

Đường cong ban đầu trong bài toán Cauchy không được trùng với một đường đặc trưng; nếu không, dữ kiện ban đầu sẽ không đủ 
để xác định duy nhất nghiệm trong một lân cận. Điều kiện này thường được viết dưới dạng:
{: .text-justify}

$$
f(x_0,y_0,w_0)\frac{dy_0}{d\tau}-g(x_0,y_0,w_0)\frac{dx_0}{d\tau}\neq 0. \tag{10}\label{10}
$$

Nếu $(\ref{10})$ bị vi phạm thì đường cong ban đầu là *đặc trưng* của PDE. Khi đó có thể không tồn tại nghiệm, hoặc tồn tại 
nhưng không duy nhất.
{: .text-justify}

## Nhận xét

Phương pháp đặc trưng là một trong những công cụ cơ bản nhất cho PDE bậc nhất. Bản chất của nó là thay bài toán vi phân từng 
phần bằng một bài toán vi phân thường trên những đường đặc biệt mà dọc theo đó PDE trở nên đơn giản hơn rất nhiều.
{: .text-justify}

Đối với các PDE bậc cao hơn, đặc biệt là các phương trình elliptic, parabolic và hyperbolic bậc hai, cấu trúc lý thuyết sẽ 
phong phú hơn đáng kể và nghiệm thường phải đi kèm với điều kiện biên, điều kiện đầu hoặc các không gian hàm thích hợp.
{: .text-justify}

<div align="center">.</div>

<div align="right"><i>(Hết)</i></div>
---
