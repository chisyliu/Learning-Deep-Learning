# [M3D-RPN: Monocular 3D Region Proposal Network for Object Detection](https://arxiv.org/abs/1907.06038)

_October 2019_

tl;dr: Regress 2D and 3D bbox parameters simultaneously by precomputing 3D mean stats for each 2D anchor. 

#### Overall impression
M3D RPN directly regresses 2D and 3D bboxes (11 + num_class), similar to [SS3D](ss3d.md) which directly regresses 26 numbers.

The algorithm requires 3D GT in the first place, and requires accurate intrinsics. For dataset without intrinsics, it may be necessary to predict intrinsics as a weakly supervised problem.

The paper focuses on a single-stage 2D and 3D regression task, and yields less accurate results particularly for yaw (postprocessing leads to 5% accuracy increase). This is evidenced by the post-processing yaw adjustment stage. It takes additional 18 ms, too slow for real time application.

The paper is correct that many previous SOTA algorithm uses pretrained component and they sometimes introduce constant noise in training.

#### Key ideas
- To ease the 3D bbox regression task, the mean stats of depth and 3D size, and theta are precomputed for each anchor size. --> This is clever, but may not be the best way to do this as the theta is quite inaccurate and calls for post-processing as shown by the paper. 
- Depth aware convolution: The depth is largely correlated with rows in autonomous driving scenes. Thus M3D RPN proposes to use separate conv filters for different row bins.

#### Technical details
- The depth aware conv leads to depth-aware features or local features and are fused with normal conv (global features) by a learned paramter. 
- For each of the 12 regression target, a learned parameter + sigmoid is used to dynamically weigh the global features and local features.

#### Notes
- Maybe giving yaw angle a larger weight in regression will alleviate the problem.