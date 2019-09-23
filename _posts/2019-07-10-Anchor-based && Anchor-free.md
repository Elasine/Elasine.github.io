# Anchor-based && Anchor-free

## CornerNet : Detecting Objects as Paired Keypoints

### Motivation

1. 通过关键点检测目标
2. Corner pooling,可有效定位边角

### Method

CornerNet直接预测目标边界框的左上角和右下角的一对定点，使用单一卷积模型生成热点图的连接矢量。

![1569227982047](../img/1569227982047.png)

#### Corner pooling 可用于定位边角

![1569228827216](../img/1569228827216.png)

#### 模型架构

![1569228869591](../img/1569228869591.png)

#### 损失函数

##### 分类损失函数

![1569228975582](../img/1569228975582.png)

##### 回归损失函数

![1569228999453](../img/1569228999453.png)

#### 分离边角

![1569229028551](../img/1569229028551.png)

### Conclusion

1. 无需设置anchor
2. Corner pooling 用于提取热点图，嵌入向量（使得相同目标的两个顶点的距离最短），偏移（用于生产更紧密的边界定位框）

## Feature Selection Anchor-Free Module for Single-Shot Object Detection (FSAF)

### Motivation

1. 如何自动学习合适的特征图做预测？

   ![1569224991831](../img/1569224991831.png)

### Method

以RetinaNet为主要结构，添加一个FSAF分支和原来的分类，回归分支并行，在不改变原有的基础上实现完全的端对端训练，框架如下图：

![1569225041645](../img/1569225041645.png)

### Ground-truth 和 loss的设计

分类分支是Focal loss

![1569227082077](../img/1569227082077.png)

回归分支是Iou loss

![1569227098492](../img/1569227098492.png)

### 在线特征选择

![1569226662776](../img/1569226662776.png)

![1569227016469](../img/1569227016469.png)

### Conclusion

1. 动态选择特征层进行目标的检测输出
2. 同时使用anchor-based和anchor-free分支

## CenterNet

### Motivation

1. 直接检测目标的中心点和大小

### Method

网络输出3个预测值，80个类，2个预测的中心点坐标，2个中心点偏置

损失函数：




