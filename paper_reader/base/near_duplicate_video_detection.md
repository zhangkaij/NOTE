[Quora](https://www.quora.com/How-can-I-compute-the-similarity-of-two-video-files-for-video-retrieval)

## Method 1
You should do feature pooling to standardize number of features for each video:
* Extract features from each keyframes (i.e if N keyframes, we will have $N*M$ sized vectors), then take their mean along temporal dimension: so we will represent each video with an 1xM sized feature vector
* Another choice is to take the max response for each keyframe, and then take their mean

## Method 2
The question is highly related a very challenging research topic: near duplicate video detection.

The popular idea is to compare frame-level similarities using features, such as SIFT. The results can be further improved with temporal smoothing and geometry verification.
