---
layout: post
title: 'COMPACT: Convolutional Auto-Encoders for Neural Image Compression'
date: 2022-11-14 04:44 +0000
categories: [Data Compression, Computer Vision]
tags: [machine learning, pytorch, images]
toc: true
author: zayn_rekhi
---

*Abstract* — Images play an essential role in the digital world. They are vital to the way we communicate and enhance the way we store information. Traditional image compression algorithms were created with the purpose of transmitting and storing images more efficiently. By compressing images to a smaller size, it improves the speed at which it is transmitted and minimizes its storage space. However, recent growth in image data caused by social media and internet browsing warrants more robust solutions. This paper proposes a deep learning-based image compression codec (a compression and decompression algorithm) called COMPACT that utilizes convolutional auto-encoders and stochastic binarization to improve image storage and transmission. The underlying structure is built on an encoder and decoder framework. An encoder is used to reduce the size of the original image into a compressed representation. When it needs to be accessed, a decoder reconstructs the image into its original representation. Testing has shown that COMPACT compresses images to sizes that are 11x smaller than the industry standard JPEG and 7x smaller than WebP (another prevalent image compression codec), while preserving similar standards of image quality. By compressing images beyond the abilities of traditional algorithms, COMPACT can provide faster internet connections as well as reduce the environmental impact of physical storage devices.

-----


### Background

Images are being transmitted faster than ever before. With social media alone accounting for 3.2 billion images transmitted daily and internet browsing accounting for many more [1]. By improving the efficacy by which images are transmitted, less bandwidth is needed to transmit them, which results in faster internet speeds. 

![Desktop View](https://ik.imagekit.io/8r3hzmyge/Image_Compression_-_GeneralArchitecture__1_.png?ik-sdk-version=javascript-1.4.3&updatedAt=1668401967522){: width="972" height="589" }

_**Figure 1.** 	The original un-compressed image on the left is inputted into the encoder to produce the compressed image. When the image needs to be accessed again, the decoder is used to reconstruct the original image from the compressed one._

The problem of transmission is coupled with storage. Every image that is transmitted needs a location at which it is stored. Hence, the steep growth in the transmission of image data has also been accompanied with an increase in physical storage devices such as SSD (Solid State Drives) and HDD (Hard Disks Drives). However, SSDs and HDDs emit large amount of carbon into the atmosphere, making them terrible for the environment [2]. Compressing images to smaller sizes results in the usage of less physical storage devices, decreasing the effect of SSDs and HDDs.

Traditional image compression codecs are composed of 2 components: the encoder and decoder. The sender uses the encoder to transform the original image into a compressed image. The receiver uses the decoder to reconstruct the original image as accurately as possible from the compressed image. Hence, the sender and the receiver can view the same image while internet bandwidth and storage space are saved (see Fig 1).                                                                                 
### COMPACT Design

At the core of COMPACT, is the CAE (Convolutional Auto Encoder) architecture. CAEs are a form of generative modelling constructed of 3 main components: encoder, compressed representation, and decoder. Additionally, a process known as stochastic binarization is placed in between the encoder and compressed representation to transform it into binary. Fig. 2 illustrates this architecture.

![Desktop View](https://ik.imagekit.io/8r3hzmyge/Image_Compression_-_ModelArchitecture__2_.png?ik-sdk-version=javascript-1.4.3&updatedAt=1668402454168){: width="972" height="589" }

_**Figure 2.** 	The input image is fed into the encoder (towards the top) which produces the compressed representation. This is fed into stochastic binarization (towards the right) before it is transmitted. After transmitted, the decoder (towards the bottom) reconstructs the image._

The encoder is composed of 6 convolutional layers that progressively decrease in size with GDN (Generalized Divisive Normalization) activation functions placed in between them. Progressively decreasing the size of the convolutional layers not only yields a smaller sized representation of the input but preserves the most important features from the original image and discards the rest. This is vital for the quality of the reconstructed image as more accurate inferences can be made from a compressed representation that preserves the most important elements of the original image.

Once the encoder produces the compressed image, the process of stochastic binarization is used to transform its floating-point values into a binary form. 

The decoder is the inverse of the encoder. It is composed of 6 up-convolutional layers that progressively increase in size with GDN activation functions placed in between them. It takes as input the binary values of the compressed representation to reconstruct the original image.

### Methodology

COMPACT is trained and evaluated on the Youtube-8M dataset: a collection of randomly selected 720p images created by the Video Understanding Group within Google Research [3]. Due to hardware limitations, a random sample of 2,286 images was selected from the original dataset. The model was created using Pytorch and training was conducted on an Apple Silicon GPU for a total of 2 epochs lasting around 13 hours. During training, the Adam optimizer was employed as well as a batch size of 8 samples. Additionally, an initial learning rate of 0.00005 “decayed” (reduced to smaller values) as training progressed to ensure the convergence of model parameters.

### Results and Discussion

Compression codecs need to satisfy two main performance requirements: high compression ratio (measures the size of the uncompressed image against the size of the compressed image) and preservation of quality.    

To ensure that the model produces image that are smaller than the traditional image compression algorithms, all the images in the dataset were compressed via COMPACT, stored locally, and compared with JPEG [4] and WebP [5]. Fig. 3 shows that COMPACT took 7.5 KB (kilobytes) of storage per image, whereas JPEG and WebP took 81.2 KB and 52.5 KB respectively. This illustrates that COMPACT compresses images almost 11x smaller than JPEG and 7x smaller than WebP. Not to mention the uncompressed format took 2800 KB, producing a compression ratio of 373:1, which is higher than all its competitors. 

![Desktop View](https://ik.imagekit.io/8r3hzmyge/Picture1.png?ik-sdk-version=javascript-1.4.3&updatedAt=1668402647661){: width="972" height="589" }

_**Figure 3.** COMPACT, JPEG, and WebP are compared between the size of their average compressed image storage space. COMPACT is the smallest at 7.5KB, then WebP at 52.5 KB, and finally JPEG at 81.2 KB._

Fig. 4 applies the COMPACT architecture to a random image in the Youtube-8M dataset. In comparison to current image compression frameworks, JPEG and WebP, COMPACT preserves similar quality. In fact, the image it produces in comparison to other frameworks is almost indistinguishable.

![Desktop View](https://ik.imagekit.io/8r3hzmyge/Image_Compression_-_Comparison_Table__2_.png?ik-sdk-version=javascript-1.4.3&updatedAt=1668402391703){: width="972" height="589" }

_**Figure 4.** 	The original image (top left), COMPACT image (top right), JPEG image (bottom left), and WebP image (bottom right) are compared._

### Conclusion

This study introduces COMPACT: a deep learning-based image compression codec which produced compression ratios that are beyond the ability of traditional codecs, while preserving a similar level of quality. In an era where images are a staple in most people’s daily lives, COMPACT aims to optimize their interaction with electronic devices by efficiently compressing images. Furthermore, this technology could greatly reduce the environmental impact of physical storage devices and enhance the speed at which images are transmitted.