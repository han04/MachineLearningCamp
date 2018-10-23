---
title: SMO
date: 2018-10-18 23:26:53
tags:
mathjax: true
---

### **回顾SVM优化目标函数**
我们先回顾下我们的优化目标函数：
$\underbrace{ min }_{\alpha}  \frac{1}{2}\sum\limits_{i=1,j=1}^{m}\alpha_i\alpha_jy_iy_jK(x_i,x_j) - \sum\limits_{i=1}^{m}\alpha_i$
我们的解要满足的KKT条件的对偶互补条件为：
$\alpha_{i}^{*}(y_i(w^Tx_i + b) - 1 + \xi_i^{*}) = 0$
根据这个KKT条件的对偶互补条件，我们有：
$\alpha_{i}^{*} = 0 \Rightarrow y_i(w^{*} \bullet \phi(x_i) + b) \geq 1$
$\alpha_{i}^{*} = 0 \Rightarrow y_i(w^{*} \bullet \phi(x_i) + b) \geq 1$
$\alpha_{i}^{*}= C \Rightarrow y_i(w^{*} \bullet \phi(x_i) + b) \leq 1$
由于$w^{*} = \sum\limits_{j=1}^{m}\alpha_j^{*}y_j\phi(x_j)$我们令$g(x) = w^{*} \bullet \phi(x) + b =\sum\limits_{j=1}^{m}\alpha_j^{*}y_jK(x, x_j)+ b^{*}$
则有：
$\alpha_{i}^{*} = 0 \Rightarrow y_ig(x_i) \geq 1$
$0 < \alpha_{i}^{*} < C  \Rightarrow y_ig(x_i)  = 1$
$\alpha_{i}^{*}= C \Rightarrow y_ig(x_i)  \leq 1$
### **SMO算法基本思想**
上面这个优化式子比较复杂，里面有m个变量组成的向量α需要在目标函数极小化的时候求出。直接优化时很难的。SMO算法则采用了一种启发式的方法。它每次只优化两个变量，将其他的变量都视为常数。由于$\sum\limits_{i=1}^{m}\alpha_iy_i = 0$假如将α3,α4,...,αm　固定，那么α1,α2之间的关系也确定了。这样SMO算法将一个复杂的优化算法转化为一个比较简单的两变量优化问题。
  为了后面表示方便，我们定义$K_{ij} = \phi(x_i) \bullet \phi(x_j)$
  由于$\alpha_3, \alpha_4, ..., \alpha_m$都成了常量，所有的常量我们都从目标函数去除，这样我们上一节的目标优化函数变成下式：
$\;\underbrace{ min }_{\alpha_1, \alpha_1} \frac{1}{2}K_{11}\alpha_1^2 + \frac{1}{2}K_{22}\alpha_2^2 +y_1y_2K_{12}\alpha_1 \alpha_2 -(\alpha_1 + \alpha_2) +y_1\alpha_1\sum\limits_{i=3}^{m}y_i\alpha_iK_{i1} + y_2\alpha_2\sum\limits_{i=3}^{m}y_i\alpha_iK_{i2}$
$s.t. \;\;\alpha_1y_1 +  \alpha_2y_2 = -\sum\limits_{i=3}^{m}y_i\alpha_i = \varsigma$
$0 \leq \alpha_i \leq C \;\; i =1,2$