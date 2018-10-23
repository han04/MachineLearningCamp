---
title: SMO
date: 2018-10-18T23:26:53.000Z
tags: null
mathjax: true
---  
  
  
### **回顾SVM优化目标函数**
  
我们先回顾下我们的优化目标函数：
<img src="https://latex.codecogs.com/gif.latex?&#x5C;underbrace{%20min%20}_{&#x5C;alpha}%20%20&#x5C;frac{1}{2}&#x5C;sum&#x5C;limits_{i=1,j=1}^{m}&#x5C;alpha_i&#x5C;alpha_jy_iy_jK(x_i,x_j)%20-%20&#x5C;sum&#x5C;limits_{i=1}^{m}&#x5C;alpha_i"/>
我们的解要满足的KKT条件的对偶互补条件为：
<img src="https://latex.codecogs.com/gif.latex?&#x5C;alpha_{i}^{*}(y_i(w^Tx_i%20+%20b)%20-%201%20+%20&#x5C;xi_i^{*})%20=%200"/>
根据这个KKT条件的对偶互补条件，我们有：
<img src="https://latex.codecogs.com/gif.latex?&#x5C;alpha_{i}^{*}%20=%200%20&#x5C;Rightarrow%20y_i(w^{*}%20&#x5C;bullet%20&#x5C;phi(x_i)%20+%20b)%20&#x5C;geq%201"/>
<img src="https://latex.codecogs.com/gif.latex?&#x5C;alpha_{i}^{*}%20=%200%20&#x5C;Rightarrow%20y_i(w^{*}%20&#x5C;bullet%20&#x5C;phi(x_i)%20+%20b)%20&#x5C;geq%201"/>
<img src="https://latex.codecogs.com/gif.latex?&#x5C;alpha_{i}^{*}=%20C%20&#x5C;Rightarrow%20y_i(w^{*}%20&#x5C;bullet%20&#x5C;phi(x_i)%20+%20b)%20&#x5C;leq%201"/>
由于<img src="https://latex.codecogs.com/gif.latex?w^{*}%20=%20&#x5C;sum&#x5C;limits_{j=1}^{m}&#x5C;alpha_j^{*}y_j&#x5C;phi(x_j)"/>我们令<img src="https://latex.codecogs.com/gif.latex?g(x)%20=%20w^{*}%20&#x5C;bullet%20&#x5C;phi(x)%20+%20b%20=&#x5C;sum&#x5C;limits_{j=1}^{m}&#x5C;alpha_j^{*}y_jK(x,%20x_j)+%20b^{*}"/>
则有：
<img src="https://latex.codecogs.com/gif.latex?&#x5C;alpha_{i}^{*}%20=%200%20&#x5C;Rightarrow%20y_ig(x_i)%20&#x5C;geq%201"/>
<img src="https://latex.codecogs.com/gif.latex?0%20&lt;%20&#x5C;alpha_{i}^{*}%20&lt;%20C%20%20&#x5C;Rightarrow%20y_ig(x_i)%20%20=%201"/>
<img src="https://latex.codecogs.com/gif.latex?&#x5C;alpha_{i}^{*}=%20C%20&#x5C;Rightarrow%20y_ig(x_i)%20%20&#x5C;leq%201"/>
### **SMO算法基本思想**
  
上面这个优化式子比较复杂，里面有m个变量组成的向量α需要在目标函数极小化的时候求出。直接优化时很难的。SMO算法则采用了一种启发式的方法。它每次只优化两个变量，将其他的变量都视为常数。由于<img src="https://latex.codecogs.com/gif.latex?&#x5C;sum&#x5C;limits_{i=1}^{m}&#x5C;alpha_iy_i%20=%200"/>假如将α3,α4,...,αm　固定，那么α1,α2之间的关系也确定了。这样SMO算法将一个复杂的优化算法转化为一个比较简单的两变量优化问题。
  为了后面表示方便，我们定义<img src="https://latex.codecogs.com/gif.latex?K_{ij}%20=%20&#x5C;phi(x_i)%20&#x5C;bullet%20&#x5C;phi(x_j)"/>
  由于<img src="https://latex.codecogs.com/gif.latex?&#x5C;alpha_3,%20&#x5C;alpha_4,%20...,%20&#x5C;alpha_m"/>都成了常量，所有的常量我们都从目标函数去除，这样我们上一节的目标优化函数变成下式：
<img src="https://latex.codecogs.com/gif.latex?&#x5C;;&#x5C;underbrace{%20min%20}_{&#x5C;alpha_1,%20&#x5C;alpha_1}%20&#x5C;frac{1}{2}K_{11}&#x5C;alpha_1^2%20+%20&#x5C;frac{1}{2}K_{22}&#x5C;alpha_2^2%20+y_1y_2K_{12}&#x5C;alpha_1%20&#x5C;alpha_2%20-(&#x5C;alpha_1%20+%20&#x5C;alpha_2)%20+y_1&#x5C;alpha_1&#x5C;sum&#x5C;limits_{i=3}^{m}y_i&#x5C;alpha_iK_{i1}%20+%20y_2&#x5C;alpha_2&#x5C;sum&#x5C;limits_{i=3}^{m}y_i&#x5C;alpha_iK_{i2}"/>
<img src="https://latex.codecogs.com/gif.latex?s.t.%20&#x5C;;&#x5C;;&#x5C;alpha_1y_1%20+%20%20&#x5C;alpha_2y_2%20=%20-&#x5C;sum&#x5C;limits_{i=3}^{m}y_i&#x5C;alpha_i%20=%20&#x5C;varsigma"/>
<img src="https://latex.codecogs.com/gif.latex?0%20&#x5C;leq%20&#x5C;alpha_i%20&#x5C;leq%20C%20&#x5C;;&#x5C;;%20i%20=1,2"/>
  