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
Many architectures in deep learning have been demonstrated to contain redundant operations and parameters that when discarded from the model show no significant impact in prediction accuracy. Considering this V. Nekrasov et al. has investigated the existing encoder-decoder fully convolutional network architecture, RefineNet, identifying and pruning redundancies as they are found. This work contributed primarily towards the goal of real time semantic segmentation on high resolution input; reporting competitive accuracies on known benchmarking image sets at 55 FPS on 512x512 inputs.
</p>

RefineNet Architecture
======================
<p style='text-align: justify;'>
Like many fully convolutional networks RefineNet refers to the structure of the decoder network. The encoder is a pre-trained Convolutional Neural Network with its fully connected classification layers removed. This allows different back bone structures to be incorporated into the RefineNet as encoders to server different applications. In this work changes to the RefineNet's own structure are considered and the encoder network left as is. Several different encoder networks where used in order to test if there was dependence between the modifications made to the decoder implicitly in the encoder, as well as the possibility of pairing the reduced complexity RefineNet with other low-weight backbone networks, namely NASNet-Mobile and Mobile-Net-v2 to produce even lower inference times.
</p>
<p style='text-align: justify;'>
The Decoder itself is built from three units Residual Convolutional Unit (RCU), Chained Residual Pooling (CRP) and Fusion blocks in addition to a final classification layer that reduces the dimensionality of the incoming channels so that they match the number of classes. These moduels act orthogonally to the encoder network, with skip connects for outputs at different layers are present. The units act on these intermediate outputs pre-processing, up sampling and fusing shallower encoder predictions with deep ones to produce the overall output. The final combination of these predictions is then passed through the final dimensionality reduction layer to form the final size matched pixel per pixel classification of the input.
</p>

Light-Weight RefineNEt
======================
<p style='text-align: justify;'>
By changing the convolution layers' kernel size from 3x3 to 1x1 in RefineNets up sampling pathways, while modifying the RCUs to an hour glass layering (1x1, 3x3, 1x1) the number of parameters in the model is reduced by more than a half.  This modification to the kernel sizes also allows the complete removal of RCUs from the network without loss of segmentation ability; further decreasing the number of learned parameters. Making these modifications enabled average inference time to decrease from 60.25ms to 35.82 ms without loss of mean Intersection of Union on both the NYUDv2 and PASCAL Person-Part image sets using inputs of size 625x468.
</p>

Discussion Points
==================

Receptive Field Size
--------------------
<p style='text-align: justify;'>
In general, the kernel size of a convolutional layer will limit the number of global features that can meaningfully be extracted by it, reduced the effective receptive field of the network. From this understanding it would be logical to conclude that reducing the kernel size of the convolution layers from 3x3 to 1x1 the network should have a reduced ability to locate objects within the scene when re-creating the labelled output. However, in this case the reduction of kernel sizes in the network shows no significant effect on the observed receptive field or the overall performance of the network when applied to segmentation tasks.
</p>
<p style='text-align: justify;'>
Although surprising, the decoding pathway may not require or be responsible for the primary information regarding the localisation of objects in a scene. Considering the FCNs first explored by Long et al. did not include convolution layers in skip connection pre-processing, besides a single layer used to match the number of channels between tensors, still retained placement of objects within scenes. This might indicate that a large portion of the global features responsible for the recetpive field originate from the encoder network. The orthogonal decoder network serving as a pre-processing transform that adjusts the balance of components being added from the different depths that already contain the information  relating to global context. 
</p>
<p style='text-align: justify;'>
This would be supported by the interpretation of a 1x1 convolution being a pixel wise transform. However, the effect of the max pooling layer may serve as a way to interpret the kernels transformation over a wider context. Perhaps this is better viewed in the Fusion block where a 1x1 convolution is followed by up sampling, disseminating the pixel-wise transform into adjacent values as part of the interpolation method used. In this way 1x1 convolutions with different post up/down-sampling layers might be able to be interpreted as convolution layers of a larger kernel size.  
</p>

Omission of RCUs
----------------
<p style='text-align: justify;'>
It was observed that the removal of Residual Convolution Units from the light-weight network had no significant impact on segmentation performance. However, when the same modification is made to the original RefineNet structure a drop of more than 5% in accuracy is observed.
</p>
<p style='text-align: justify;'>
In structure of RefineNet the RCUs act as further simple encoding layers with a skip from input to output, as found in ResNet. The units themselves are found in two distinct locations, applied raw back bone output or as a post/pre-processing layer for CRP and Fusion blocks respectively.
Although comparable to the original RefineNet the performance of the network with same depth back bones is decreased by approximately 1%. This might be due to some information being encoded by the RCUs that can only be meaningfully included into predictions when larger kernel sizes are used. To further understand the interaction and effect of the RCUs ablation of them at different locations in the orthogonal network would determine if the smaller kernels make post-CRP units ineffectual or if there is true some additional encoding the RCUs are able to provide when acting upon intermediate backbone output or if the extra encoding is more useful at certain depths than others.
</p>