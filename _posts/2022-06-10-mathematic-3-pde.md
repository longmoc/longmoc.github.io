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

*Phương trình vi phân thông thường chỉ mô tả hệ động học một chiều trong khi các hệ động học trong tự nhiên (các hiện 
tượng chuyển động của âm thanh, nhiệt, sự phân tán tĩnh điện học...) hầu hết là các hệ động học đa chiều, biến đổi trong 
không gian và thời gian. Các hệ động học này được mô tả thông qua **Phương trình vi phân từng phần (Partial Differential 
Equation - PDE)**. Công cụ toán học trở thành cầu nối liên hệ và tổng quát hóa nhiều hiện tượng tự nhiên*
{: .text-justify}

---

## Partial Differential Equation

Một PDE là một phương trình toán học có liên quan đến hai hoặc nhiều biến độc lập, một hàm đa biến chưa biết (phụ thuộc 
vào các biến đó) và các đạo hàm riêng của hàm trên các biến độc lập tương ứng. Bậc của PDE là bậc cao nhất của đạo hàm 
có liên quan. Một *nghiệm* (hay một *nghiệm riêng* - *particular solution*) của PDE là một *hàm* mà phương trình xác định khi ta thay hàm số đó vào. 
Một nghiệm được gọi là *nghiệm chung* (*general solution*) nếu chứa tất cả các nghiệm riêng của phương trình.
{: .text-justify}

Một nghiệm đúng (*exact solution*) là nghiệm riêng của phương trình vi phân từng phần phi tuyến bậc cao (bậc hai hoặc cao hơn).
{: .text-justify}

## First-Order Partial Differential Equation

### General Form

Một *phương trình vi phân từng phần bậc nhất* (*First-order PDE*) với $n$ biến độc lập có dạng chuẩn:
{: .text-justify}

$$F\biggl(x_1,x_2,\dots, x_n,w,\frac{\partial w}{\partial x_1},
\frac{\partial w}{\partial x_2},\dots,\frac{\partial w}{\partial x_n}\biggl)=0$$

với $w=w(x_1,x_2,\dots, x_n)$ là hàm đa biến chưa biết và $F(\dots)$ là hàm số đã cho.
{: .text-justify}

### Quasilinear Equations, Characteristic System, General Solution

### Quasilinear Equations

Một *phương trình vi phân từng phần tựa tuyến tính bậc nhất* (*first-order quasi-linear PDE*) có hai biến độc lập sẽ có dạng chung:
{: .text-justify}

$$ f(x,y,w)\frac{\partial w}{\partial x}+g(x,y,w)\frac{\partial w}{\partial y}=h(x,y,w). \tag{1}\label{1}$$

Các phương trình dạng này được gặp trong nhiều ứng dụng khác nhau như cơ học liên tục, khí động học, thủy động lực học, 
dẫn truyền nhiệt, lý thuyết sóng, âm học, dòng chảy nhiều pha, kỹ thuật hóa học... 
{: .text-justify}

Nếu các hàm $f$, $g$ và $h$ độc lập so với $w$ chưa biết thì phương trình $(\ref{1})$ là *tuyến tính* (*linear*).
{: .text-justify}

#### Characteristic system. General solution

Hệ các phương trình vi phân thông thường
{: .text-justify}

$$\frac{dx}{f(x,y,w)}=\frac{dy}{g(x,y,w)}=\frac{dw}{h(x,y,w)} \tag{2}\label{2}$$

là *hệ phương trình đặc trưng* (*characteristic system*) của phương trình $(\ref{1})$. Giả sử tìm được hai nghiệm riêng 
độc lập của hệ này có dạng:
{: .text-justify}

$$\tag{3}\label{3}
u_{1}(x, y, w)=C_{1},\qquad u_{2}(x,y,w)=C_{2},$$

với $C_1$ và $C_2$ là hằng số bất kỳ. Nghiệm riêng đó gọi là tích phân của hệ phương trình $(\ref{2})$. Khi đó có thể viết 
nghiệm chung của phương trình $(\ref{1})$:
{: .text-justify}

$$\tag{4}\label{4}
\Phi(u_{1},u_{2})=0$$

trong đó $\Phi$ là một hàm bất kỳ của hai biến.
{: .text-justify}

<div align="center">.</div>
---