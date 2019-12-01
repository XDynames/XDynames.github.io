---
layout: post
title:  "Deep Learning Architectures for Image Classification"
date:   2019-12-01
excerpt: "A sequenced reading list for understanding common network design features for computer vison tasks"
tag: Image Classification, Deep Learning
comments: false
---



[AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)
------------
<p style='text-align: justify;'>
First demonstration of the power of deeplearning on image classification tasks. Contains five convolutional layers, interweaved with three max pooling operations and three fully-connected layers to form predictions with. Showcases the benefits of ReLU and Dropout.
</p>

[VGG](https://arxiv.org/abs/1409.1556)
--------
<p style='text-align: justify;'>
Following a similar pattern to AlexNet, VGG introduces a refined design pattern for CNNs and explores possible configurations. This introduced a popular design pattern where blocks of sequential convolution layers are followed by max pooling; with each block containing more channels than the previous.
</p>

[GoogleNet (Inception v1)](https://ai.google/research/pubs/pub43022)
--------------------------
<p style='text-align: justify;'>
Uses multiple-kernel sizes in convolutional blocks assembled in parallel to capture multi-scale information in images.
</p>

[SqueezeNet](https://arxiv.org/abs/1602.07360)
---------------
<p style='text-align: justify;'>
Creation of a model with AlexNet like preformance, with a fraction of the learnable paramters. Achived via preferencing 1x1 kernels in a design pattern refered to as Fire modules and the introduction of bypass connections around some modules.
</p>

[ResNet](https://arxiv.org/abs/1512.03385)
---------------
<p style='text-align: justify;'>
Demonstrated that by including skip connections between convolution blocks networks that are much deeper are able to be trained without issues of gradient vanishing. Despite their depth these models contain less learnable parameters than previous state of the art models like VGG. Another case where the bottleneck pattern seen primitively in SqueezeNet is seen with 1x1 convolutions being used before and after 3x3s within residual blocks.
</p>

[Inception v2 & 3](https://arxiv.org/abs/1512.00567)
--------------------------
<p style='text-align: justify;'>
Optimisation of the incpetion modules is preformed by factorising larger kerneled convolution braches into sequences of smaller kernel layers.
</p>

[Inception v5](https://arxiv.org/abs/1602.07261)
--------------------------
<p style='text-align: justify;'>
Uses a hybrid network between ResNet and Inception v3 to inform the design of a purely inception based network with preformance matching the combined model.
</p>

[ResNeXt](https://arxiv.org/abs/1611.05431)
---------------
<p style='text-align: justify;'>
Fusing the ideas presented in Inception and ResNet, ResNeXt uses multiple parallel convolutional bottlenecks with different kernel sizes to form residual inception modules. Also demonstrates the degeneracy present between Inception-ResNet and Grouped Convolution operations.
</p>

[MobileNet](https://arxiv.org/abs/1704.04861)
---------------
<p style='text-align: justify;'>

</p>

[MobileNet v2](https://arxiv.org/abs/1801.04381)
---------------
<p style='text-align: justify;'>

</p>