2017/09/13 早晨 周三

## 在十一前有如下工作要做：

1. 继续调整基于SSD网络结构的算法；

2. 复现yolo实验，现在别人复现的没有达到论文的准确率；

3. 基于yolo网络结构，验证我的算法，看看一般化能力；

4. 复现tiny yolo，然后利用SRGAN的思想，看看能不能提升tiny yolo的准确率；

5. 复现Mask RCNN。



## 现在工作进度情况：

1. 工作1正在进行中，现在准确率为74.2，比论文中的准确率低3%。

2. 工作2完成后再开始工作3和工作4，工作2的现状为已经有人复现，但是比论文低2%，需要找到原因。----预计9月17号前完成工作2

3. 工作5的工作量较大，现在没有官方源码，大家组建了一个小组共同复现。 ----预计9月17号开始复现



## 想法：

1. 对于工作4，初步想法为：先训练好yolo，其次训练好tiny yolo，然后使用GAN，对抗训练tiny yolo。

# 2017/09/25 晚上 周一
注我们的算法简称为SSDN
## 工作进度总结
1. 我们的算法准确率为77.58，正确超过SSD，至少78.5吧，目标再提高一个1%
2. yolo的代码还在跑，最近仍然以提高我们算法的准确率为主，附带调试这个网络结构。

## 工作计划：
SSDN待验证点：
1、nms的阈值为0.5，再测试一组0.6和0.4的效果。
2、IoU大于0.5的为正样本A，小于0.3的为负样本B。其中正样本经过过滤后剩下的为最佳样本C。现在训练loss的时候，A和C反向传播，但是并没有区分A和A-C部分，进行相同权重的BP。如果A-C部分的权重略小于C部分的权重，会产生什么样的效果。

## 想法：
1、为了解决小样本的问题，希望使用VGG16前面层的feature map。但是由于见面层的feature map较大，会产生大量的anchor----其中绝大部分为无效anchor。faster RCNN为two-stage，先产生anchor，对anchor进行训练，然后把每一个anchor映射到feature map，提取特征。但是SSD和YOLO为为one-stage，它没有利用anchor包围的区域的feature map来训练分类器的过程，假设也就没有必要一个feature map像素对应一个，为了解决former feature map提取特征的能力较差，可以使用跨层连接以提高其性能--可以参考RON的网络结构。

# 26/9/2017 周二
# 上午
## 想法
1. 2017年CVPR阿里一篇文章关于图片关键物体检测（有可能是分割），可以利用SSD的最后三层feature map，分别输出5*5、3*3和1*1大小的feature map，就能检测到关键物体。参考论文Self-explanatory Deep Salient Object Detection

test
![](https://github.com/zhangkaij/myBlog/blob/gh-pages/images/ssd_introduction.png?raw=true)


