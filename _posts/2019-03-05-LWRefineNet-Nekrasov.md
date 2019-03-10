---
layout: post
title:  "[Reading] Light-Weight RefineNet for Semantic Segmentation"
date:   2019-03-05
excerpt: "Light-Weight RefineNet for Semantic Segmentation by Nekrasov et al."
tag: FCN, Fully Convolutional, Light-Weight, Semantic Segmentation, Segmentation
comments: false
project: true
---

Overview
========
Many archetectures in deep learning have been demonstrated to contain redundent operations and parameters that when discarded from the model show no significant impact in prediction accuracy. In light of this V. Nekrasov et al. has investigated the existing encoder-decoder fully convolutional network archiecture, RefineNet, identifying and pruning redundancies as they are found. This work contributed primarily towards the goal of real time semantic segmentation on high resolution input; reprorting competitive accuracies on known benchmarking image sets at 55 FPS on 512x512 inputs.