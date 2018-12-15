# YOLO的优缺点
## 疑问
- [ ] batchsize为64，没有说图片大小是多大？
## introduction
* unlike sliding window and region proposal-based techniques, YOLO sees the entire image during training and test time so it implicitly encodes contextual information about classes as well as their appearance. Fast R-CNN, a top detection method, mistakes background patches in an image for objeccts because it cann't see the larger context. YOLO makes less than half the number of background errors compared to Fast R-CNN.
如果网络能够看到整张图片，背景识别的准确率会提高。YOLO 在背景识别方面比Fast RCNN高，为什么平均准确率却比Fast RCNN低？
* YOLO的缺点，不能准确识别出较小的物体。

## unified detection
* we use a batchsize of 64, a momentum of 0.9 and a decay of 0.0005.



