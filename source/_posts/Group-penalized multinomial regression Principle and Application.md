#  Group-penalized multinomial regression Principle and Application
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
------

> * 套索回归（Lasso）
> * 基于分组惩罚的线性回归
> * 基于分组惩罚的多响应Lasso回归
> * 多项式回归
> * 基于分组惩罚的多项式回归(GPMR)
> * 基于GPMR高光谱波段选择应用
> * 参考文献

<!--more-->
## 套索回归（Lasso）
对于一般的线性回归来说，具有$n$个响应值的向量$y$，和每个响应值对应的$p$维特征的样本$X_i$，建立输入$X$与输出$y$之间的回归关系，通常使用最小二乘法求解参数$\beta$。同时为了使得模型具有解析解并防止模型过拟合，加入模型惩罚项。
$$\hat \beta {\rm{ = }}\arg {\min _\beta }\frac{1}{2}\left\| {y - X\beta } \right\|_2^2 + \lambda {\left\| \beta  \right\|_1}$$
（使用latex公式符号，会导致deploy出现error失败，还未解决。。。）


## 参考文献
[1]. 李航. 统计学习方法[M]. 清华大学出版社, 北京, 2012.
[2]. Mitchell T M. 机器学习[M]. 机械工业出版社, 2003.
[3]. Trevor Hastie and Junyang Qian. Glmnet Vignette.[N/OL] http://web.stanford.edu/~hastie/glmnet/glmnet_beta.html	
