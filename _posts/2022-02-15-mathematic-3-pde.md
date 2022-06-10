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

---

## Partial Differential Equation

Một PDE là một phương trình toán học có liên quan đến hai hoặc nhiều biến độc lập, một hàm đa biến chưa biết (phụ thuộc 
vào các biến đó) và các đạo hàm riêng của hàm trên các biến độc lập tương ứng. Bậc của PDE là bậc cao nhất của đạo hàm 
có liên quan. Một *nghiệm* (hay một *nghiệm riêng* - *particular solution*) của PDE là một *hàm* mà phương trình xác định khi ta thay hàm số đó vào. 
Một nghiệm được gọi là *nghiệm chung* (*general solution*) nếu chứa tất cả các nghiệm riêng của phương trình.

Một nghiệm đúng (*exact solution*) là nghiệm riêng của phương trình vi phân từng phần phi tuyến bậc cao (bậc hai hoặc cao hơn).

## First-Order Partial Differential Equation

### General Form

Một phương trình vi phân từng phần bậc nhất (First-order PDE) với $n$ biến độc lập có dạng chuẩn:

$$F\biggl(x_1,x_2,\dots, x_n,w,\frac{\partial w}{\partial x_1},
\frac{\partial w}{\partial x_2},\dots,\frac{\partial w}{\partial x_n}\biggl)=0$$

với $w=w(x_1,x_2,\dots, x_n)$ là hàm đa biến chưa biết và $F(\dots)$ là hàm số đã cho.

### Quasilinear Equations, Characteristic System, General Solution

Một phương trình vi phân từng phần tựa tuyến tính bậc nhất (first-order quasi-linear PDE) có hai biến độc lập sẽ có dạng chung:

$$ f(x,y,w)\frac{\partial w}{\partial x}+g(x,y,w)\frac{\partial w}{\partial y}=h(x,y,w). \tag{1}\label{1}$$


<div align="center">.</div> 

<div align="right"><i>(Hết)</i></div> 
---