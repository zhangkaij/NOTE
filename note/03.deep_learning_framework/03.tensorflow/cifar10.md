![kai](https://github.com/zhangkaij/paper_img/blob/master/deep_learn_framework/tensorflow/Parallelism.png?raw=true)

可以看到，每个GPU会用一批独立的数据计算梯度和估计值。这种假设非常有效的将一大批数据分割到各个gpu上。
这一机制要求所有的GPU能够共享模型参数。但是众所周知在GPU之间传输数据非常慢，因此我们决定在CPU上存储和更新所有模型的参数。这样以来，GPU在处理一批新的数据之前会跟新一遍参数。