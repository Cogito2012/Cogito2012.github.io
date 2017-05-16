# SCI论文数据可视化--联合使用Group和Stack绘制柱状图

------
解决的问题包括：
> * 复杂柱状图的控制
> * SCI论文图表常见配色方案
> * 图例横排设置

<!--more-->
先上效果图：
<center>
![Graph](http://cogito2012.github.io/assets/img/graph.bmp "图1 联合使用Group和Stack绘制柱状图")
</center>

然后结合MATLAB代码，逐步说明实现效果吧。

数据说明：
柱状图下方的4个数据列分别为p1,p2,p3,p4,上方的四个数据列分别为b1,b2,b3,b4。这8个数据列均为13维向量。

思路分析一：
上述柱状图，从上下叠加的角度来看，存在上下两组13×4的数据矩阵，即对于每组数据来说，有4个13元素的数据列需要绘制成柱状图。横坐标绘图范围为（4+1）×13=65，这里+1表示把13组柱状图之间的间隔，看做高度为0的柱状图。于是，图表可以拆解为4组上下stack起来的bar柱状图，第一组横坐标位置为1:5:61，第二组2:5:62，第三组3:5:63，第四组4:5:64，第五组（全零）5:5:65.
所以，只需要构造2个65维列向量，使用MATLAB的bar函数的stack属性，就可以实现上下叠加了。
```python
N = 13；
% 下方数据，4×13
X = [p1;p2;p3;p4];
% 补充0数据行，5×13
data = cat(1,X,zeros(1,N));
% reshape为65×1的列向量
Y1 = reshape(data,N*5,1);
% 上方数据，处理同上
B = [b1;b2;b3;b4];
data = cat(1,B,zeros(1,N));
Y2 = reshape(data,N*5,1);
% 利用bar绘制柱状图，stack方式
myBar = bar([Y1,Y2],'stack');
% 设置横轴label的范围0-65，位置依次为2:5:62
set(gca,'XLim',[0 65],'XTick',2:5:62,'XTickLabel',xticks);
% 设置填充颜色
set(myBar(1),'facecolor','c')
set(myBar(2),'facecolor','y')
```
于是效果如图：
<center>
![Graph](http://cogito2012.github.io/assets/img/graph1.bmp "图2 基本画法（配色失败）")
</center>
这时候，才发现4组柱状图的颜色没法分别控制，只能统一更改上方颜色或者下方颜色。这是由于上方和下方数据都是作为整体，同时绘制在柱状图上的，figure的bar句柄对象myBar只有2个成员。

所以，需要分别对四组数据进行绘制。
只画第1组数据时，2,3,4组数据位置填充0；只画第2组数据时，1,3,4组数据位置填充0；只画第3组数据时，1,2,4组数据位置填充0；只画第4组数据时，1,2,3组数据位置填充0；

所以，更改的代码如下：
```python
clear;
clc;
close all
figure
set(gcf,'Position',[50,50,1050,450])
xticks = {'AIM', 'CA', 'COV', 'FES', 'GBVS', 'MC', 'PCA', 'RBD',  'SeR', 'SIM', 'SUN', 'SWD', 'GR'};
N = 13;

% load('bar3.mat');
% 模拟数据
p1 = round(rand(1,N)*10)+1;
p2 = round(rand(1,N)*10)+1;
p3 = round(rand(1,N)*10)+1;
p4 = round(rand(1,N)*10)+1;
b1 = ones(1,N);
b2 = 2*ones(1,N);
b3 = 3*ones(1,N);
b4 = 4*ones(1,N);

%% =============绘制第1组===================
X = [p1;p2;p3;p4];
B = [b1;b2;b3;b4];
X(2:4,:) = 0; % 第2、3、4组数据置零
B(2:4,:) = 0;
data = cat(1,X,zeros(1,N));
Y1 = reshape(data,N*5,1);
data = cat(1,B,zeros(1,N));
Y2 = reshape(data,N*5,1);

barH1 = bar([Y1,Y2],'stack');
set(gca,'XLim',[0 65],'XTick',2:5:62,'XTickLabel',xticks);
% ch = get(myBar,'children');
set(barH1(1),'facecolor',[38,54,129]./255)
set(barH1(2),'facecolor',[255,217,47]./255)
hold on

%%=============绘制第2组===================
X = [p1;p2;p3;p4];
B = [b1;b2;b3;b4];
X([1,3,4],:) = 0; % 第1、3、4组数据置零
B([1,3,4],:) = 0;
data = cat(1,X,zeros(1,N));
Y1 = reshape(data,N*5,1);
data = cat(1,B,zeros(1,N));
Y2 = reshape(data,N*5,1);

barH2 = bar([Y1,Y2],'stack');
set(gca,'XLim',[0 65],'XTick',2:5:62,'XTickLabel',xticks);
% ch = get(myBar,'children');
set(barH2(1),'facecolor',[104,191,193]./255)
set(barH2(2),'facecolor',[255,217,47]./255)
hold on

%%=============绘制第3组===================
X = [p1;p2;p3;p4];
B = [b1;b2;b3;b4];
X([1,2,4],:) = 0; % 第1、2、4组数据置零
B([1,2,4],:) = 0;
data = cat(1,X,zeros(1,N));
Y1 = reshape(data,N*5,1);
data = cat(1,B,zeros(1,N));
Y2 = reshape(data,N*5,1);

barH3 = bar([Y1,Y2],'stack');
set(gca,'XLim',[0 65],'XTick',2:5:62,'XTickLabel',xticks);
% ch = get(myBar,'children');
set(barH3(1),'facecolor',[44,160,44]./255)
set(barH3(2),'facecolor',[255,217,47]./255)
hold on

%%=============绘制第4组===================
X = [p1;p2;p3;p4];
B = [b1;b2;b3;b4];
X([1,2,3],:) = 0;% 第1、2、3组数据置零
B([1,2,3],:) = 0;
data = cat(1,X,zeros(1,N));
Y1 = reshape(data,N*5,1);
data = cat(1,B,zeros(1,N));
Y2 = reshape(data,N*5,1);

barH4 = bar([Y1,Y2],'stack');
set(gca,'XLim',[0 65],'XTick',2:5:62,'XTickLabel',xticks);
% ch = get(myBar,'children');
set(barH4(1),'facecolor',[0,184,229]./255)
set(barH4(2),'facecolor',[255,217,47]./255)
hold on

set(gca,'FontSize',18,'FontName','Times New Roman')
legendH = legend([barH1(1),barH2(1),barH3(1),barH4(1),barH4(2)],{'CC','sAUC','AUC\_J','NSS','increase'});
% 设置legend水平摆放，防止覆盖图表
set(legendH,'Orientation','horizon')

```
最终效果如图：

<center>
![Graph](http://cogito2012.github.io/assets/img/graph.bmp "图3 联合使用Group和Stack绘制柱状图")
</center>

最后附上SCI论文比较常用的配色图，更多配图方案参考: [图表的基本配色方法](https://zhuanlan.zhihu.com/p/23377067)
<center>
![Graph](http://cogito2012.github.io/assets/img/sci_color.png "图4 SCI论文比较常用的配色图")
</center>
