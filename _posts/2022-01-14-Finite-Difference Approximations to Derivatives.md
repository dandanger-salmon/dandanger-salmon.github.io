---
layout:     post
title:      Finite-Difference Approximations to Derivatives
subtitle:   Finite-Difference Approximations to Derivatives
date:       2022-01-14
author:     ZYC
catalog: true
comments: true
tags:
    - NWP
    - Python
    - Exercise
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

# 第二章 导数的有限差分近似
- 整理时间：2022年01月14日
- 整理人：张赟程

## 2.1 有限差分商
1. **有限差分方法：** 通过在指定（空间或时间）区域中有限数量的离散点处定义的一组值来表示连续函数$f(x)$。
2. **有限差商：**
   - **$j$点前向差商：**
  $$ 
  \left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )_{j,approx}=\frac{f_{j+1}-f_{j}}{\Delta x}　　　　（1）
  $$ 
   - **$j$点后项差商：**
  $$ 
  \left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )_{j,approx}=\frac{f_{j}-f_{j-1}}{\Delta x}　　　　（2）
  $$
   - **$（j+\frac{1}{2}）$点中央差商：**
  $$ 
  \left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )_{j+\frac{1}{2},approx}=\frac{f_{j+1}-f_{j}}{\Delta x}　　　　（3）
  $$
   - **$j$点中央差商：**
  $$
  \left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )_{j,approx}=\frac{f_{j+1}-f_{j-1}}{2\Delta x}　　　　（4） 
  $$ 
  **两点近似：** 使用了$f$中两点的值，并且求取了其中一点处导数$\left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )$的近似值。如公式(1)(2)。
  **三点近似：** 求取导数$\left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )$近似值的点不同于在$f$中的取值点。如公式(3)(4)。
  当x为时间时，则称为“level”,可称为两级近似和三级近似。
3. **有限差分近似的准确性：** 其中一种度量为**截断误差**，指的是无限级数展开的截断。
  在$x_{j}$处$f$的泰勒级数展开，
  $$
  f_{j+1}=f_{j}+\Delta x\left (\frac{\mathrm{df} }{\mathrm{d} x}  \right )_{j}+\frac{(\Delta x)^{2}}{2!}\left ( \frac{\mathrm{d^{2}} f}{\mathrm{d} x^{2}} \right )_{j}+\frac{(\Delta x)^{3}}{3!}\left ( \frac{\mathrm{d^{3}} f}{\mathrm{d} x^{3}} \right )_{j}+\cdots +\frac{(\Delta x)^{n-1}}{(n-1)!}\left ( \frac{\mathrm{d^{n-1}} f}{\mathrm{d} x^{n-1}} \right )_{j}+\cdots
  $$ 
  改写为
  $$
  \frac{f_{j+1}-f_{j}}{\Delta x}=\left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )_{j}+\varepsilon 
  $$
  其中$\varepsilon$称为截断误差
  $$
  \varepsilon=\frac{(\Delta x)}{2!}\left ( \frac{\mathrm{d^{2}} f}{\mathrm{d} x^{2}} \right )_{j}+\frac{(\Delta x)^{2}}{3!}\left ( \frac{\mathrm{d^{3}} f}{\mathrm{d} x^{3}} \right )_{j}+\cdots +\frac{(\Delta x)^{n-2}}{(n-1)!}\left ( \frac{\mathrm{d^{n-1}} f}{\mathrm{d} x^{n-1}} \right )_{j}+\cdots
  $$
  在$\varepsilon$中，当$\Delta x$趋于无穷小，第一项最大。可称为一阶近似或者一阶精度近似。一阶导数的一阶近似方案可记为：
  $$
  \left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )_{j,approx}=  \left ( \frac{\mathrm{d} f}{\mathrm{d} x} \right )_{j}+O[x]
  $$
