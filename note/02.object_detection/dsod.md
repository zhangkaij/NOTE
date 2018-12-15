## Abstract
&emsp;&emsp;性能最优的目标检测算法首先在大规模分类数据集上训练网络，然后在目标检测数据集上对网络进行微调。但是把在分类数据集上训练的模型转换成不同领域的目标检测模型非常困难，比如从RGB图像到深度图像。作者提出DSOD，直接在目标检测数据集上训练模型，不需要在分类数据集上预训练。
## Introduction
&emsp;&emsp;为了获得更好的性能，绝大多数目标检测系统都微调整在Imagenet预训练的网络。微调整过程也被称作**transfer learning**。微调整过程至少有如下两个优点：
* 大量的预训练深度模型可以使用，使用它们进行目标检测比较方便；
* 缩短训练过程，适用于训练集规模较小的情况。

使用预训练的网络缺点也很明显：
* 现在目标检测系统直接使用预训练的网络，限制网络结构的修改；
* 分类和目标检测任务的loss函数和数据分布都不同，会导致不同的优化/搜索空间。使用预训练网络极大可能导致局部最优；
* domain mismatch:如果源域（ource domain）与目标域（target domain）严重的不匹配时，比如从RGB图像到深度图像、医疗图像等，现在的目标检测算法效果很差。

作者的工作：
* 是否可以在目标检测数据集上直接训练模型，不需要经过预训练？
* 如果可以在目标检测数据上直接训练模型，那么遵循哪些规则？

注：在作者贡献的一系列规则中，其中**deep supervision**最重要，受文献[18,35]的启发。

## Related work
目标检测方法可以分为两类：
* region proposal based methods，比如R-CNN,Fast R-CNN,Faster R-CNN和R-FCN。
* proposal-free methods,比如Yolo,SSD。

经典网络结构：AlexNet,VGGnet,GoogLeNet,ResNet,DenseNet。

## DSOD
规则：
* **proposal free**:只有proposal free网络结构才能收敛；
* **Deep supervision**在GoogLeNet[32],DSN[18],DeepID3[30]中已经证明深度监督学习具有较好的效果。它的主要思想为设计一个**integrated objective function**对前面的隐藏层提供直接监督。该方法可以减弱梯度消失问题。通过side-output layers来实现，类似于文献[35]。
* **Stem Block**：使用较小卷积核（**3*3**）获得较高的准确率。相比DenseNet的**7*7**卷积核可以保存更多的细节信息。
* **Dense Prediction Structure**:使用先MaxPooling后卷积层结构的目的为了降低计算量。