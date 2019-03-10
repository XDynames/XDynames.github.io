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
<p style='text-align: justify;'>
Many archetectures in deep learning have been demonstrated to contain redundent operations and parameters that when discarded from the model show no significant impact in prediction accuracy. In light of this V. Nekrasov et al. has investigated the existing encoder-decoder fully convolutional network archiecture, RefineNet, identifying and pruning redundancies as they are found. This work contributed primarily towards the goal of real time semantic segmentation on high resolution input; reprorting competitive accuracies on known benchmarking image sets at 55 FPS on 512x512 inputs.
</p>

RefineNet Architecture
======================
<p style='text-align: justify;'>
Like many fully convolutional networks RefineNet referes to the structure of the decoder network. The encoder is a pre-trained Convolutionalion Neural Network with its fully connected classification layers removed. This allows different back bone structures to be incorperated into the RefineNet as encoders to server different applications. In this work changes to the RefineNet's own structure are considered and the encoder nertwork left as is. Several different encoder networks where used in order to test if there was dependance between the modifications made to the decoder implicity in the encoder, as well as the possibility of pairing the reduced complexity RefineNet with other low-wieght backbone netoworks, namely NASNet-Mobile and Mobile-Net-v2 to produce even lower inference times.
</p>
<p style='text-align: justify;'>
The Decoder itself is built from three units Residual Convoloutional Unit (RCU), Chained Residual Pooling (CRP) and Fusion blocks in addition to a final classification layer that reduces the dimensionality of the incoming channels so that they match the number of classes. These moduels act orthoganoly to the encoder network, with skip conects for outputs at different layers are present. The units act on these intermiediate outputs pre-processing, upsampling and fusing shallower encoder predicitions with deep ones to produce the overall output. The final combination of these predictions is then passed through the final dimensionality reduction layer to form the final size matched pixel per pixel classification of the input.
</p>

Light-Weight RefineNEt
======================
<p style='text-align: justify;'>
By changing the convolution layers' kernel size from 3x3 to 1x1 in RefineNets upsampling pathways, while modifying the RCUs to an hour glass layering (1x1, 3x3, 1x1) the number of paramters in the model is reduced by more than a half. In general the kernel size of a convolutional layer will limit the amount of global features that can meaningfully be extracted by it, reduced the effective receptive field of the network. However in this case the refactoring of the network in this manner shows no significant effect on the observed receptive field or the overall preformance of the network when applied to segmentation tasks. This modification to the kernel sizes also allows the complete removal of RCUs from the network without loss of segmentation ability; further decreasing the number of learned parameters. Making these modifications eanbled average inference time to decreae from 60.25ms to 35.82 ms without loss of mean Intersection of Union on both the NYUDv2 and PASCAL Person-Part image sets using inputs of size 625x468.
</p>

Disscussion Points
==================
<p style='text-align: justify;'>

</p>