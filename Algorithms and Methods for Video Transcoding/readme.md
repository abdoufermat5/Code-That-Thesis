# Algorithms and Methods for Video Transcoding

_by Akshay Shashidhara Nagaraghatta_

## Summary of Abstract

Video transcoding is the process of dynamic video adaptation, which can be defined as the process of converting video from one format to another, changing the bit rate, frame rate or resolution of the encoded video, which is mainly necessitated by the end user requirements.

Until 2013s, the H.264 compression standard was the most widely used video codec. However, the H.265 compression standard, also known as High Efficiency Video Coding (HEVC), has been developed to provide a better Rate-Distortion (RD) performance than H.264. However, the computational complexity of H.265 is much higher than that of H.264, which makes it difficult to use in real-time applications.

This make it necessary to develop low complexity video transcoding algorithms to transcode from H.264 to HEVC.

The current paper by Akshay Shashidhara Nagaraghatta proposes three different video transcoding algorithms:

1. **The MV based mode merge algorithm**, which uses the motion vectors (MVs) of the H.264 encoded video to transcode it to HEVC.
2. **The conditional probability based mode mapping**, which uses probability models to predict the block sizes in HEVC (e.g., 16x16 or smaller) based on the block modes and quantization parameters (QP) from the H.264 video.
3. **The motion compensated MB Residual based mode mapping**, which uses the motion compensated macroblock (MB) residuals from the H.264 video to predict the block sizes in HEVC. Residual data refers to the difference between the predicted and actual video frame. The algorithm adapts based on the video content to decide how to split the video blocks efficiently.

> As a result, the proposed algorithms have reduced the computational complexity of the HEVC encoder by around 60% with negligible loss in rate-distortion performance.

## Introduction

### Problem Statement

The very high penetration of video applications in day-to-day life brings with it a greater need for inter-operability of video content on different devices. The original video content may be stored in a server in a specific video format at a particular resolution. This needs to be converted into different combinations of video resolution and formats to be compatible with the end user's requirements.

For example, Youtube stores the video in 240p, 360p, 480p, 720p, 1080p, 1440p and 2160p to cater to different user requirements. 

The network bandwidth and the device capabilities are the main factors that determine the video resolution and format that can be played on the end user device.

As the device capabilities and network propertioes are unknown at the time of video compression, the video transcoding process (resolution, bitrate) is necessary to adapt the video content to the end user requirements.

HEVC offers around 2x times better compression of video data compared to H.264. For example a H.264 compressed video which needs an internet bandwidth of 1Mbps can be compressed to 0.5Mbps (500Kbps) using HEVC, and that's same ratio for storage space.

**To support high quality videos over constrained networks, and for efficient storage of video content, and for supporting newer devices which do not support legacy video formats, there is a need to transcode the video content from H.264 to HEVC.**

The most straightforward way to transcode video from H.264 to HEVC is to decode the H.264 decoder and then encode it using HEVC. However the most commonly used approach is to re-use the decoded information for the subsequent stage of encoding to reduce the computational complexity of the encoder.

The largest block size of HEVC standard is 64x64, while the largest block size of H.264 is 16x16. This makes it difficult to directly map the H.264 blocks to HEVC blocks.

Motion estimation is the most complex stage during the process of encoding.

While H.264 allows for 259 different partitioning combinations within a 16x16 macroblock, HEVC's 64x64 Coding Tree Units can be partitioned in over 3^64 ways, even when considering only three modes (8×8, 8×4 and 4×8) for each 8×8 block. It is impossible to achieve real time encoding in HEVC with cost effective hardware.

**This make the development of low complexity algorithms for HEVC encoding very important.**
