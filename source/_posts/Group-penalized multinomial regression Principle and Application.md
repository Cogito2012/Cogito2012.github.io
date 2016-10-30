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
## 基于分组惩罚的线性回归
上述模型的前提是各个特征分量相关性较小的情形，当不同的特征分量高度相关时，将所有的特征分量分成$k$个不同的分组，不同特征组之间相关性小，组内相关性大，于是就得到了基于分组惩罚的线性回归（group-penalized linear regression）问题。
$$\hat \beta {\rm{ = }}\arg {\min _\beta }\frac{1}{2}\left\| {y - X\beta } \right\|_2^2 + \lambda \sum\limits_k {{{\left\| {{\beta _{I(k)}}} \right\|}_2}} $$
需要注意，这个模型的惩罚项看似是$L2$范数正则化惩罚项，但它是对组内变量求$L2$范数，对组间是一个求和运算，而求和运算的作用与$L1$正则化类似，于是达到了组间参数的稀疏性。
## 基于分组惩罚的多响应Lasso回归
进一步往下思考，上述模型中的样本标签$y$是一个$n$维列向量，表示$n$个样本对应的响应值，比如类别标签值，每项响应值为一个。当响应值为$M$个时，比如用0-1之间的值表示每个样本在$M$个不同类别上响应概率的大小，于是$Y$为$n\times M$矩阵，$\beta$为$p\times M$矩阵，模型就转变为基于分组惩罚的多响应Lasso回归（group-penalized multiresponse lasso regression）：
$$\hat \beta {\rm{ = }}\arg {\min _\beta }\frac{1}{2}\left\| {Y - X\beta } \right\|_F^2 + \lambda \sum\limits_{k = 1}^p {{{\left\| {{\beta _{k}}} \right\|}_2}} $$
## 多项式回归
那么接下来的问题是，如何将多响应回归模型推广到多项式回归（multinomial regression）呢?根据广义线性模型（GLM）的基本原理，多项式回归问题中首先对于一个给定的样本$x$，求该样本属于类别l的后验概率为：
$$P(y = l\left| x \right.) = \frac{{\exp ({x^{\rm{T}}}{\beta _{l}})}}{{\sum\nolimits_{m \le M} {\exp ({x^{\rm{T}}}{\beta _{m}})} }}$$
于是，类比线性回归模型，我们希望通过最小化一个类似于如下的目标函数，使得参数$\beta$具有分组的稀疏性：
$$\hat \beta {\rm{ = }}\arg {\min _\beta } - \ell (y,X\beta ) + \lambda \sum\limits_{k = 1}^p {{{\left\| {{\beta _{k}}} \right\|}_2}} $$
也就是说对于$p\times M$的参数$\beta$，由于只有当第$k$行所有元素为0时函数才可微，因此当模型取得极小值时得到的$\beta$参数矩阵将有较多的零行向量，于是这个稀疏的$\beta$解实现了基于multinomial的特征选择过程。同时，上述每个样本在各个类别上的后验概率同时计算得出。

因此基于分组的多项式回归模型在进行特征选择的同时，也完成了基于选择的特征进行多项式分类器的训练过程，两个过程是一体化的。这一点与常规的特征选择思路有很大的不同，因为常规方法通常是先进行特征选择，然后用选择的特征进行分类器训练，进而评价特征选择算法的性能。

于是现在只剩最后一个问题，就是如何表达上述最优化模型等式右边第一项的问题，也就是对经验风险$\ell (y,X\beta )$的建模。当完成了上述公式中的$\ell (y,X\beta )$建模后，上述公式也就是基于分组惩罚的多项式回归(GPMR)。

## 基于分组惩罚的多项式回归(GPMR)
在多项式广义线性模型中，假定$n$个样本，特征维度为$p$，每个样本$i$在$M$个类别上分别具有单一响应值输出，那么观测样本属于类别$m$的后验概率为：
$${p_{i,m}} = \frac{{\exp ({\eta _{i,m}})}}{{\sum\nolimits_{l \le M} {\exp ({\eta _{i,l}})} }}$$
其中：
$${\eta _{i,m}} = {X_{i}}{\beta _{m}}$$
于是将经验风险表达为最大化多项式对数似然：
$$\ell (p) = \sum\limits_{i = 1}^n {\sum\limits_{m = 1}^M {{y_{i,m}}\log ({p_{i,m}})} } $$
将后验概率带入到似然函数，进行函数化简可得到：
$$\begin{array}{c}
\ell (\eta ) = \sum\limits_{i = 1}^n {\sum\limits_{m = 1}^M {{y_{i,m}}\left[ {{\eta _{i,m}} - \log \left( {\sum\nolimits_{l \le M} {\exp ({\eta _{i,l}})} } \right)} \right]} } \\
 = \sum\limits_{i = 1}^n {\left[ {\sum\limits_{m = 1}^M {{y_{i,m}}{\eta _{i,m}} - \log \left( {\sum\nolimits_{l \le M} {\exp ({\eta _{i,l}})} } \right)} } \right]} 
\end{array}$$
考虑到每个样本在$M$个类别上的响应值的和为1这样的事实，即有：
$$\sum\limits_{m = 1}^M {{y_{i,m}}}  = 1$$
因此，最大化上述似然函数等价于最小化如下添加负号的模型：
$$\min  - \sum\limits_{i = 1}^n {\left[ {\sum\limits_{m = 1}^M {{y_{i,m}}{X_{i}}{\beta _{m}} - \log \left( {\sum\nolimits_{l \le M} {\exp ({X_{i}}{\beta _{l}})} } \right)} } \right]}  + \lambda \sum\limits_{k \le p} {{{\left\| {{\beta _{k}}} \right\|}_2}} $$
其中右边的惩罚项实现了参数beta的稀疏。于是这个模型第一项就是上述我们要建立的经验风险模型。至此便完成了最终的基于分组惩罚的多项式套索回归模型的建立。

而关于模型的求解，Noah Simon等人于2013年提出了分块梯度下降算法来实现模型的训练和参数求解，并且附有详尽的数学公式推导和定理证明，在此不作详述，作者已将这套算法用R语言和MATLAB写入了用于求解lasso回归问题的glmnet工具包，见文献[6]。
## 基于GPMR高光谱波段选择应用
我最近做的高光谱波段选择系列实验，就是使用glmnet来实现这套算法的应用。以Indian Pines高光谱数据为例，训练数据包含有185个波段的600个训练样本，每个样本被标记为16个类别中的某一类。如图是其中一个类别的样本波谱统计曲线。可以看到在不同的波段区间，灰度响应值呈曲线变化，且相邻波段高度相关。
<center>
![band spectrum curve](http://cogito2012.github.io/assets/img/bandspectrum.png "图1 兔耕玉米地类样本波谱曲线")
</center>
而波段选择的目的，就是寻求一组最优的波段能够最优的刻画这种波谱分布，从而利用低维波谱数据在地物分类任务上的精度与全体高维波段数据精度相当，并且极大减少算法计算量。令600×185的训练样本数据为$X$，600×1的列向量为$Y$，通过上述模型训练，得到的一个模型参数$\beta$随参数$\lambda$的变化图示为：
<center>
![Coefficients Lambda curve](http://cogito2012.github.io/assets/img/Coff_Lambda.png "图2 参数稀疏性随惩罚系数变化曲线")
</center>
表明，随着惩罚系数$\lambda$逐渐增大，参数beta的非零项逐渐减少，也就是约束性越来越强，这就直观的体现了通过参数稀疏实现波段选择的过程。

通过交叉验证（cross-validation）的方式，可以得到一组最优的$\lambda$，使得模型在尽量简单的情况下具有最好的分类精度。如图11为误分类误差曲线随惩罚系数$\lambda$的变化曲线。
<center>
![Misclassification Error Lambda curve](http://cogito2012.github.io/assets/img/MisError_Lambda.png "图3 误分类误差随惩罚系数变化曲线")
</center>

## 参考文献
[1]. 李航. 统计学习方法[M]. 清华大学出版社, 北京, 2012.
[2]. Mitchell T M. 机器学习[M]. 机械工业出版社, 2003.
[3]. Trevor Hastie and Junyang Qian. Glmnet Vignette.[N/OL] http://web.stanford.edu/~hastie/glmnet/glmnet_beta.html	
