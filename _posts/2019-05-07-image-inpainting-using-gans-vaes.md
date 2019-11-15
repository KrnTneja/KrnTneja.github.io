---
title:  "Semantic Image Inpainting for Medical Images with Deep Generative Models"
date:   2019-05-07
permalink: /posts/2019/image-inpaiting-gans-vaes
tags:
  - course-project
  - generative-models
  - generative-adversarial-networks
  - variational-autoencoders
  - image
  - image-inpainting
---

I took the course <i>Medical Image Computing</i> in Spring 2019 offered by [Prof. Suyash Awate](https://www.cse.iitb.ac.in/~suyash/) at IIT Bombay. This is a blog post about the work I did for the course project with Pranav Kulkarni on using GANs for image inpainting based on work by R. Yeh et al. in CVPR 2017 and extending it to use VAE for the same problem and showing that VAEs perform better than GANs for our task. 

The motivation behind image inpainting for medical images comes from need of pre-processing images with lesions for downstream tasks like segmentation or registration. The idea behind using deep generative model is to use strong priors and use the given context to understand high-level features to fill the missing regions in the images. An example on natural image is an shown below where inpainting algorithm must derive the information about characteristics from partial face and use prior knowledge to construct the missing part of face. 

![](/images/image-inpaint-example.png)

The goal is to generate the missing content by conditioning on the available data by using generative models (like GANs) where the generator acts as a mapping from latent space to images. Our method is a re-implementation and extension of from work by R. Yeh et al. in CVPR 2017. Once our generative model is trained and produces realistic images from random vectors in the latent space, the pipeline for inpainting is followed which has following main three main steps:
1. For inpainting, we find closest encoding of the corrupted image in latent space using context loss and prior loss. This is explained in detail below.
2. Pass the encoding through the generative model to infer missing content.
3. Blend the predicted patch intensities to have coherence with surrounding known pixel intensities using blending.

The inference in this method is possible independent of the structure of missing part. We don't need to account for the and size of corrupted patches while training the model. This work has provided realistic state of the art results for image inpainting on face images.

First step needs more clarification here. The latent encoding of the corrupted image is found by using backpropogation to the input. Recall that the input to a generator is a random Gaussian vector with zero mean and unit variance. Using context loss ensures that the encoding of the input actually produces the known content in the output when passed through the generative model. Prior loss is used to ensure that the output image is indeed realistic.

Let $G$ be the generator and $D$ be the discriminator. The context loss is calculated by minimizing the masked $L_1$ norm between the given image $y$ and output image $G(z)$ where $z$ in the estimated latent encoding i.e. for a given mask $M$, context loss is $L_c(z\|y,M) = \|\|W \odot (G(z)-y)\|\|_1$. In the paper, they've suggested that giving more weight to pixels on the boundary of missing region improves the output and we confirmed it in our experiments as well.

Since there is missing portions in image from which we are deriving the latent vector representation, we need to use the prior loss to ensure realistic outputs. In GANs, this is done by using the discriminator. The prior loss would therefore be $L_p(z) = \lambda \log (1- D(G(z)))$ where $\lambda$ is the prior weight. 

Though this process may give reasonable and well-aligned inpainted image, the predicted pixels may not exactly preserve the same intensities of the surrounding pixels. To correct this, authors have used Poisson blending in the third step. Poisson blending has the following objective function:

<center>$\hat{x} = \arg \min_x = \| \nabla x - \nabla G(\hat{z}) \|_2^2$   such that $x_i = y_i$ for $M_i = 1$</center>
<br>
The problem turns out to be equivalent to minimizing the norm of difference of Laplacian of $x$ and $G(z)$ and has a uniqe solution. We've confirmed in our experiments that this step is indeed very crucial for high quality of final output images. 

We wish to extend their work for VAEs, but VAEs where we don't have a discriminator. We need another prior loss, most intuitive way is to use penalize the norm of $z$ vector itself since its prior is a zero mean unit variance Gaussian distribition. Therefore, our prior loss is $L_p = \lambda \|\| z \|\|_2^2$. 

The slides from our project presentation can be found [here](/files/sp2019-image-inpainting.pdf).

<h2>Results</h2> 
We compare VAEs and GANs in two settings, with convolutional kernel size of 3 and 7 on structure MRI slices. The results shown below show that model indeed performs well giving high PSNR (peak signal-to-noise ratio). 

<img src="/images/inpainting-result1.png">

Inpainted images are visually almost indifferentiable. So we zoom into the missing region the image below and observe how VAEs are performing better and larger convolutional kernels help.

<img src="/images/inpainting-result2.png">

We have done many more experiments which are present in the slides below. In summary, we have shown that VAEs are consistently better in terms of PSNR and SSIM (structural similarity), even for larger patches though more details are missing in every method that we tried. Further, in the image shown below, we see the importance of prior loss, weighting of context loss and blending by doing an ablation study. The red circle shows a particular example of how weighted context loss plays an important role in image inpainting performance. 

<img src="/images/inpainting-result3.png">
 
An important observation that we made is that the effect of prior loss in GANs using discriminator is not very prominent, and there are many examples where not using prior gave a better result. A snapshot of this observation is shown below. 

<img src="/images/inpainting-result4.png">

<h2>Comments and Conclusions</h2>
Main question that we need to answer is: <b>why are VAEs performing better than GANs?</b> One plausible reason is that VAEs are explicitly trained to produce real images while generator is GANs is learned indirectly from the gradients flowing from discriminator. This can lead to stronger prior influence in VAEs than GANs. Further, in our particular problem, the modes of variations are much lesser than natural images. Because of explicit training, the VAEs can learn the primary modes of variations early in the training. Further an imporant advantage of VAEs in our setting is that they were trained in 25% less epochs, each consuming 25% less time. Overall VAEs were 78% faster to train. 

In our project we extended the ideas from above mentioned paper to use VAE for image inpainting on medical images. We showed that VAEs consistently performed better than GANs in terms of PSNR and SSIM metrics and also observed the same visually. We confirmed the importance of prior loss, weighted context loss and blending and observed that the advantage due to prior loss is more clearly observed in VAEs than GANs.


<!-- Attention-based models belong to a class of models commonly called sequence-to-sequence models. The aim of these models, as name suggests, it to produce an output sequence given an input sequence which are, in general, of different lengths. Let input sequence be $\mathbf{x} = \\{x_1, x_2, \ldots, x_T\\}$ and output sequence be $\mathbf{y} = \\{y_1, y_2, \ldots, y_U\\}$. There are 2 parts of a sequence-to-sequence models viz. encoder and decoder, both of which are RNNs. Encoder encodes the given input producing a series of hidden states $\mathbf{h} = \\{h_1, h_2, \ldots, h_T\\}$ of input $\mathbf{x}$.

&&\mathbf{h} = Encoder(\mathbf{x})&&

![](/images/encoder_new.png)
*Bi-directional RNN based encoder takes the input sequence $\mathbf{x} = \\{x_1, x_2, \ldots, x_T\\}$ outputs the hidden states $\mathbf{h} = \\{h_1, h_2, \ldots, h_T\\}$ (memory vectors).*

<h2>References</h2>

<b id='enc-dec'>Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Translation</b><br>
Kyunghyun Cho, Bart van Merrienboer, Caglar Gulcehre, Dzmitry Bahdanau, Fethi Bougares, Holger Schwenk, Yoshua Bengio <br>
Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP)

<b id='2lstm'>Sequence to Sequence Learning with Neural Networks</b><br>
Ilya Sutskever, Oriol Vinyals, Quoc V. Le <br>
Proceedings of the 27th International Conference on Neural Information Processing Systems (NIPS 2014)

<b id='ctc'>Connectionist Temporal Classification: Labelling Unsegmented Sequence Data with Recurrent Neural Networks</b><br>
Alex Graves, Santiago Fer<e>&aacute;</e>nandez, Faustino Gomez, J<e>&uuml;</e>rgen Schmidhuber <br>
Proceedings of the 23rd International Machine Learning Conference, 2006

<b id='soft'>Neural Machine Translation by Jointly Learning to Align and Translate</b><br>
Dzmitry Bahdanau, Kyunghyun Cho, Yoshua Bengio <br>
International Conference on Learning Representations, 2015

<b id='luong'>Effective Approaches to Attention-based Neural Machine Translation</b><br>
Minh-Thang Luong, Hieu Pham, Christopher D. Manning
Proceedings of the 2015 Conference on Empirical Methods in Natural Language Processing  

<h2>Further Reading</h2>

<b>Structured Attention Networks</b> <br>
Yoon Kim, Carl Denton, Luong Hoang, Alexander M. Rush <br> 
5th International Conference on Learning Representations, 2017

<b>Listen, Attend and Spell</b> <br>
William Chan, Navdeep Jaitly, Quoc V. Le, Oriol Vinyals <br>
2016 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)

<b>Monotonic Chunkwise Attention</b><br>
Chung-Cheng Chiu, Colin Raffel <br>
International Conference on Learning Representations, 2018

<b>Attention-Based Models for Speech Recognition</b><br>
Jan Chorowski, Dzmitry Bahdanau, Dmitriy Serdyuk, Kyunghyun Cho, Yoshua Bengio<br>
Proceedings of the 28th International Conference on Neural Information Processing Systems (NIPS 2015)

<b>Online and Linear-Time Attention by Enforcing Monotonic Alignments</b><br>
Colin Raffel, Minh-Thang Luong, Peter J. Liu, Ron J. Weiss, Douglas Eck <br>
Proceedings of the 34th International Conference on Machine Learning, 2017

<b>Attention Is All You Need</b><br>
Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, Illia Polosukhin <br>
31st Conference on Neural Information Processing Systems (NIPS 2017)

<b>State-of-the-art Speech Recognition With Sequence-to-Sequence Models</b><br>
Chung-Cheng Chiu, Tara N. Sainath, Yonghui Wu, Rohit Prabhavalkar, Patrick Nguyen, Zhifeng Chen, Anjuli Kannan, Ron J. Weiss, Kanishka Rao, Ekaterina Gonina, Navdeep Jaitly, Bo Li, Jan Chorowski, Michiel Bacchiani <br>
arXiv:1712.01769

<h2> Addtional Notes </h2>

1. In section 2.3.1, the architecture uses 4-layered LSTM model for both input-sequence reading and generating output-sequence. Long-range dependencies refer to the temporal dependencies in sequences and not the forward pass of the model.

2. In section 2.3.1, suppose an input sequence $a-b-c-d$ has to be translated to output sequence $A-B-C-D$. If input LSTM model sees $d-c-b-a$ instead of $a-b-c-d$, the output \'summary\' vector is influenced most by $a$ and hence it becomes easier to realize for output LSTM to start with $A$. Although average dependency length between the pairs ($a-b-c-d$, $A-B-C-D$) and ($d-c-b-a$, $A-B-C-D$) is same, the model probably finds it easier to start the sequence with initial $A$ and $B$ and so on. Temporal dependencies later help in generating the $C$ and $D$ more easily on average. This is very crude reasoning and there is no \'proof\' as such but this phenomenon of better performance on the input sequence reversal is observed and can be explained as above. -->