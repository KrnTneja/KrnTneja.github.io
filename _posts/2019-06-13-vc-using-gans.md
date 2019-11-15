---
title:  "Voice Conversion using GANs: An Extensive Review"
date:   2019-06-13
permalink: /posts/2019/vc-using-gans
tags:
  - voice-conversion
  - generative-adversarial-networks
  - speech
  - image
  - literature-review
---

For my project on code-mixed speech recognition with [Prof. Preethi Jyothi](https://www.cse.iitb.ac.in/~pjyothi/), I did a literature review
of voice conversion and found a lot of recent papers that used GANs for the problem. So I decided to write this post to summarize my review. I've also attached the slides and references I used in my group meeting at the end of this post. 

Back in 2015, before GANs became popular for conversion problems, voice conversion models were trained for a particular target speaker and models were trained simply on regression loss. One good example is [Lifa et al, 2015](#1) where a deep BiLSTM network is used. They use [STRAIGHT vocoder system](http://web.wakayama-u.ac.jp/~kawahara/APSIPA2014TutorialT1/APSIPA_Tutorial_Kawahara.pdf) to convert an utterance into 3 types of features viz. Mel-cepstral coefficients, aperiodicity measures and $\log F_0$. Only the Mel-ceptral coefficients are transformed by BiLSTM for voice conversion. $\log F_0$ is linearly transformed using input speaker information and aperiodicity measures were simply forwarded to synthesis system. The synthesis system took the transformed Mel-cepstral coefficients, aperiodicity measures and transformed $log F_0$ to generate output utterance. The This system had several limitations including single target speaker and need for parallel utterances which also had to be aligned using dynamic-time warping. 

Before GANs were used for voice conversion, they were explored in the image domain, very notably by [Zhu et al, 2017](#2) and [Kim et al, 2017](#3). In [Zhu et al, 2017](#2), the idea of cycle-consistency loss is introduced which makes the whole thing possible. It is best explained from the figure shown below from the same paper.

<img src="/images/cycle-consistency-loss.png">

The idea is that there are two tranformation networks $G$ and $F$, which tranform from domain $X$ to $Y$ and $Y$ to $X$ respectively. In addition to the discriminative loss for real-fake classification, a consistency loss is used to ensure that $\hat{x} = F(G(x))$ is close to $x$ itself and $\hat{y} = G(F(y)$ is close to $y$ itself. The idea is totally life-changing! In case one is not impressed enough by the elegance of this idea, their mind will surely be blown away by the amazing results they've managed to achieve. An example from the paper is shown below where paintings from different painters were treated as different domains. Even if these results don't satisfy you (who are you?), think about the big advantage that this model has: <b>there is no need of parallel data</b>. Just get a set of images from different domains, and transform them to impress your friends.

<img src="/images/image-to-image-transform.png">

The idea proposed in [Kim et al, 2017](#3) is very similar except that they use it to find relationships among datapoints different domains. An example is shown below where model learns to identify the orientation of input image of a chair and give out an image of car with same oreintation. 

<img src="/images/chair-car-transform.png" style="width:70%;">

Another exmaple shown below is for transformation between a male face and a female face.

<img src="/images/male-female-transform.png">

Extending these ideas to the voice conversion is pretty straightforward. [Kaneko et al, 2017](#4) used the same idea of cycle-consistency loss for training a voice conversion system where they treated two different speakers as two domains. For details on their gated-CNN based model and how they use features from WORLD vocoder, one should refer to the paper. They have also provided samples [here](http://www.kecl.ntt.co.jp/people/kaneko.takuhiro/projects/cyclegan-vc/index.html). Another very similar work is by [Fang et al, 2018](#5) where they've only transformed the lower order Mel-cepstrum using the GAN and copied the higher order information for synthesis owing to the argument that speaker-specific information is only present in lower order cepstrum. 

Both of the above systems still have the disadvantage that only a pair of speaker can be used for training and inference in voice conversion. This was resolved by [Kameoka et al, 2018](#6) in their model called the StarGAN. This model has 3 parts: a generator (or more aptly called a transformer), a discriminator and a domain classifier. Here the generator and discriminator are conditioned on domain by an additional input specifiying the speaker. Simplest one to train is the classifier which is trained on real data to predict the domain of the input, or simply the speaker in case of voice conversion. Discriminator is, as usual, trained to identify real and fake datapoints. The loss for training generator has four terms with scaling parameters: an adversarial loss to fool discriminator, a domain classifier loss to ensure that output belongs to the domain the generator is conditioned on, a cyclic loss to ensure reliable conversion and an identity loss for identity conversion as a regularization. The demo can be seen [here](http://www.kecl.ntt.co.jp/people/kameoka.hirokazu/Demos/stargan-vc/index.html). They've also proposed other systems which can all be seen in the list given [here](http://www.kecl.ntt.co.jp/people/kameoka.hirokazu/Demos/). A very similar work is by [Gao et al.](#7) where they've treated genders as the two domains.  

This applications of idea of cycle-consistency have been pretty impressive. Another important papers in this line is the work by [Meng et al, 2018](#8) on speech enhancement by learning noise using cycle-consistency GANs where the two domains are that of clean speech and noisy speech which is also an extension to their previous work in [Meng et al, 2018](#9) that didn't use cycle-consistency. 

Another very interesting application is presented by [Tanaka et al, 2018](#10) where they train a cycle-consistent GAN to make synthetic speech sound more natural. As one can guess, the two domains are that of synthetic speech and natural speech. They propose that this tranformation can be used as an additional processing step in the vocoder pipelines after synthesis.

Rest of post is about other related stuff that I found interesting. Since I'm talking about a generative model, it is important to mention that variational auto-encoders (VAEs) have also been used for voice conversion. An example is the work by [Kameoka et al, 2018](#11) where they maximize the mutual information between the output and domain in addition to the usual VAE loss. This maximization ensures that the encoder and decoder actually account for the conditioning on domain and maximize the usage of bottleneck to preserve content. They show, using variational information maximization, that maximizing this mutual information reduces down to adding an auxiliary classifier and using to train VAE for minimizing classification loss. The demo can be seen [here](http://www.kecl.ntt.co.jp/people/kameoka.hirokazu/Demos/acvae-vc/index.html). One intersted in their fully convolutional VAE for voice conversion should check out the paper in more detail.

Just to end this post on a positive note about GANs, I wish to mention two very popular papers that have explored the stability of GANs which have been applied in image domain. I think that it would be very interesting to see this work in improving systems for voice conversion. First one I wish to mention is the work by [Karras et al, 2018](#12) where they progressively increase layers in network over time while ensuring smooth change in dependence on new paramters, and fading in the new layers. The resolution of the image improves over each additional layer, finally leading to amazingly realistic fake images as the ones shown below. 

<img src="/images/progressive-face.png">

Second is the paper by [Mao et al, 2017](#13) where they show that using least square error on discriminator output is more stable than using cross entropy which is based on minimizing Jenson-Shannon divergence between the generator output distribition $P_G$ and the real data $P_D$. They show that minimizing the square error is instead equivalent to minimizing the Pearson $\chi^2$ divergence between $P_D + P_G$ and $2P_G$. 

All the references and some further reading (which I'll update over time) are given below. The slides from my presentation can be found [here](/files/s2019-vc-using-gans.pdf).


<!-- Attention-based models belong to a class of models commonly called sequence-to-sequence models. The aim of these models, as name suggests, it to produce an output sequence given an input sequence which are, in general, of different lengths. Let input sequence be $\mathbf{x} = \\{x_1, x_2, \ldots, x_T\\}$ and output sequence be $\mathbf{y} = \\{y_1, y_2, \ldots, y_U\\}$. There are 2 parts of a sequence-to-sequence models viz. encoder and decoder, both of which are RNNs. Encoder encodes the given input producing a series of hidden states $\mathbf{h} = \\{h_1, h_2, \ldots, h_T\\}$ of input $\mathbf{x}$.

&&\mathbf{h} = Encoder(\mathbf{x})&&

![](/images/encoder_new.png)
*Bi-directional RNN based encoder takes the input sequence $\mathbf{x} = \\{x_1, x_2, \ldots, x_T\\}$ outputs the hidden states $\mathbf{h} = \\{h_1, h_2, \ldots, h_T\\}$ (memory vectors).*
--> 

<h2>References</h2>

<b id='1'>Voice conversion using deep Bidirectional Long Short-Term Memory based Recurrent Neural Networks</b><br>
L. Sun et al., ICASSP 2015<br>

<b id='2'>Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks</b><br>
J. Zhu et al., ICCV 2017<br>

<b id='3'>Learning to Discover Cross-Domain Relations with Generative Adversarial Networks</b><br>
T. Kim et al., ICML 2017<br>

<b id='4'>Parallel-Data-Free Voice Conversion Using Cycle-Consistent Adversarial Networks</b><br>
T. Kaneko et al., arXiv preprint arXiv:1711.11293, 2017<br>

<b id='5'>High-Quality Nonparallel Voice Conversion Based on Cycle-Consistent Adversarial Network</b><br>
F. Fang et al., ICASSP 2018<br>

<b id='6'>StarGAN-VC: non-parallel many-to-many Voice Conversion Using Star Generative Adversarial Network</b><br>
H. Kameoka et al., IEEE SLT 2018<br>

<b id='7'>Voice Impersonation using Generative Adversarial Networks</b><br>
Y. Gao et al., arXiv preprint arXiv:1802.06840, 2018<br>

<b id='8'>Cycle-Consistent Speech Enhancement</b><br>
Z. Meng et al., Interspeech 2018<br>

<b id='9'>Adversarial Feature-Mapping for Speech Enhancement</b><br>
Z. Meng et al., Interspeech 2018<br>

<b id='10'>WaveCycleGAN: Synthetic-to-natural speech waveform conversion using cycle-consistent adversarial networks</b><br>
K. Tanaka et al., arXiv preprint, 2018<br>

<b id='11'>ACVAE-VC: Non-parallel Many-to-many VC  with Auxiliary Classifier VAE</b><br>
H. Kameoka et al., arXiv preprint, 2018<br>

<b id='12'>Progressive Growing of GANs for Improved Quality, Stability and Variation</b><br>
T. Karras et al., ICLR 2018<br>

<b id='13'>Least Squares Generative Adversarial Networks</b><br>
X. Mao et al., ICCV 2017<br>

<h2>Further Reading</h2>
<b id='14'>CycleGAN-VC2: Improved CycleGAN-based non-parallel voice conversion</b><br>
T. Kaneko et al., ICASSP 2019<br>

<b id='15'>WaveCycleGAN2: Time-domain neural post-filter for speech waveform generation</b><br>
K. Tanaka et al., arXiv preprint arXiv:1904.02892, 2019<br>

<b id='16'>AUTOVC: Zero-Shot Voice Style Transfer with Only Autoencoder Loss</b><br>
K. Qian et al., Proc. MLR, 2019<br>


<!--
<h2> Addtional Notes </h2>

1. In section 2.3.1, the architecture uses 4-layered LSTM model for both input-sequence reading and generating output-sequence. Long-range dependencies refer to the temporal dependencies in sequences and not the forward pass of the model.

2. In section 2.3.1, suppose an input sequence $a-b-c-d$ has to be translated to output sequence $A-B-C-D$. If input LSTM model sees $d-c-b-a$ instead of $a-b-c-d$, the output \'summary\' vector is influenced most by $a$ and hence it becomes easier to realize for output LSTM to start with $A$. Although average dependency length between the pairs ($a-b-c-d$, $A-B-C-D$) and ($d-c-b-a$, $A-B-C-D$) is same, the model probably finds it easier to start the sequence with initial $A$ and $B$ and so on. Temporal dependencies later help in generating the $C$ and $D$ more easily on average. This is very crude reasoning and there is no \'proof\' as such but this phenomenon of better performance on the input sequence reversal is observed and can be explained as above.-->