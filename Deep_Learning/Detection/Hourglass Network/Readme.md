# Stacked Hourglass Network

论文原文：[Stacked Hourglass Networks for Human Pose Estimation](https://arxiv.org/abs/1603.06937)



## 前言

​		了解到沙漏网络是在 CornerNet 中，其提出的无锚框目标检测所使用到的 Backbone 主干网络就是 Stacked Hourglass Network。回顾CornerNet，利用沙漏网络生成 Top-Left 和 Bottom-Right 角点（Corner）图，然后判断两个热点图中是否有配对的 Corner，成功配对则能借此生成一个边界框。在这个过程中我认为有意思的点是如何生成角点图，因此翻到了这篇论文。

> 这篇论文不太好懂，很多概念很难理解，只能是介绍个大概。



## 网络描述

​		stacked hourglass network 是用来对人体姿态进行检测的，在16年提出的一种改进方案，主要用于预测人体关节点，预测出来后再进行连接相应点，完成人体姿态检测，其效果如下图。

> 作者提出的方案设计不同之处主要是自下而上的处理（高分辨率到低分辨率）和自上而下的处理（低分辨率到高分辨率）的对称设计。

![](/home/brandewijn/Document/GitKraken/Footprint/Deep_Learning/Detection/Hourglass Network/Images/example.jpg)

​		沙漏网络是 hourglass network，stacked 即是多个 hourglass 进行堆叠，整体网络图如下。经过反复下采样再上采样，然后传递给下一个沙漏继续做采样，最后生成热点图。

![](/home/brandewijn/Document/GitKraken/Footprint/Deep_Learning/Detection/Hourglass Network/Images/stacked_hourglass_network.jpg)

​		单个沙漏见下图，数据流动方向是从左往右。网络首先做 4 次下采样（残差卷积+最大池化）将分辨率降至极低（4x4），在下采样前还会拷贝一份作为跳层连接用于后面上采样的特征融合（逐元素相加）。之后再做上采样（nearest nerghbor umsampling， 最近临近上采样）将分辨率还原回输入时的分辨率。最后将产出的特征图用 1x1 的 sigmoid 卷积得到热点图。

> 这里的上采样没有使用 unpooling 和 deconv，只是简单的最近临近上采样并融合低层图像（下采样时拷贝的图像）的语义特征。

![](/home/brandewijn/Document/GitKraken/Footprint/Deep_Learning/Detection/Hourglass Network/Images/hourglass_network.jpg)

​		沙漏的下采样中，每一层的卷积都是一个残差模块（如下图）。

> 这一步是结合了卷积分解和 Inception 模块的结果。
>
> 待完善。

![](/home/brandewijn/Document/GitKraken/Footprint/Deep_Learning/Detection/Hourglass Network/Images/res_part.jpg)

​		下图显示了沙漏网络间的协作。沙漏状即沙漏网络，产出的结果特征图会拷贝出一个分支用于预测热点图（下方支路，蓝色方框即是热点图），然后将热点图映射回预测前的图像维度，并与沙漏网络结果特征图融合作为下一个沙漏网络的输入。

![](/home/brandewijn/Document/GitKraken/Footprint/Deep_Learning/Detection/Hourglass Network/Images/Intermediate_supervision.jpg)

