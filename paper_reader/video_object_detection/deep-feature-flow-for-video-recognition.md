## Deep Feature Flow for Video Recognition

### Abstract
Deep convolutional neural networks have achieved great success on image recognition tasks. Yet, it is non-trivial to transfer the state-of-the-art image recognition networks to videos as per-frame evaluation is too slow and unaffordable. We present deep feature flow, a fast and accurate framework for video recognition. It runs the expensive convolutional sub-network only on **sparse key frames and propagates their deep feature maps to other frames via a flow field**. It achieves significant speedup as flow computation is **relatively fast**. The end-to-end training of the whole architecture **significantly boosts the recognition accuracy**.
