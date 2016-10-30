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
对于一般的线性回归来说，具有$n$个响应值的向量$y$，和每个响应值对应的$p$维特征的样本${{\rm{X}}_i}$，建立输入$X$与输出$y$之间的回归关系，通常使用最小二乘法求解参数$\beta$。同时为了使得模型具有解析解并防止模型过拟合，加入模型惩罚项。
$$\hat \beta {\rm{ = }}\arg {\min _\beta }\frac{1}{2}\left\| {y - X\beta } \right\|_2^2 + \lambda {\left\| \beta  \right\|_1}$$
其中的$\lambda$为惩罚系数，控制模型复杂度。上述模型对参数$\beta$使用了$L1$正则化，于是该模型被称为套索回归（Lasso Regression）。由于$L1$正则化项实现了参数$\beta$的稀疏性，也就是说，使得上述损失函数最小化的参数$\beta$的$p$个维度中，绝大部分的分量为0，少数分量非零，所以样本$X$只有少数的特征分量影响线性回归模型性能，从而达到一种特征选择的效果。因此Lasso回归广泛运用于高维数据的降维过程。

## 参考文献
[1]. 李航. 统计学习方法[M]. 清华大学出版社, 北京, 2012.
[2]. Mitchell T M. 机器学习[M]. 机械工业出版社, 2003.
[3]. Trevor Hastie and Junyang Qian. Glmnet Vignette.[N/OL] http://web.stanford.edu/~hastie/glmnet/glmnet_beta.html	
