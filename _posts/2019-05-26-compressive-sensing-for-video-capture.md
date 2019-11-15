---
title:  "Compressed Sensing and Dictionary Learning for Video Capturing"
date:   2019-05-26
permalink: /posts/2019/compressive-sensing-for-video-capture
tags:
  - course-project
  - image
---

I took the course <i>Recent Topics is Analytical Signal Processing</i> in Spring 2019 offered by [Prof. Animesh Kumar](https://www.ee.iitb.ac.in/~animesh/) at IIT Bombay. This is a blog post about the work I did for the course project with Anmol Kagrecha and Pranav Kulkarni on using compressed sensing and dictionary learning to
alleviate tradeoff between temporal and spatial resolution in videos based on work by Hitomi et al. 2011 and Liu et al. 2014 and extending it to use different sampling strategies which can improve the performance but are not necessarily constrained by hardware implementations.

<h2>Introduction</h2>
Digital cameras face a fundamental trade-off between spatial and temporal resolution. This limitation is primarily due to hardware factors such as the readout and time for analog-todigital (A/D) conversion in image sensors. The problem can be alleviated by using parallel A/D converters and frame buffers but it incurs more cost.
[Hitomi et al.](#1) and [Liu et al.](#2) propose techniques for sampling, representing and reconstructing the space-time volume in order to overcome this trade-off. We aim to propose and experiment with different sampling strategies which can improve the performance but are not necessarily constrained by hardware implementations.
The techniques used by [Hitomi et al.](#1) and [Liu et al.](#2) contain several hyper-parameters. We also aim to understand the effect of these hyper-parameters on the performance of the techniques by performing exhaustive experiments.

<h2>Overview</h2>
Denote the space-time volume corresponding to an $M\times M$ pixel neighborhood and one frame integration time of the camera as $E(x,y,t)$. A projection of this volume along the time dimension is captured by a camera. A $N$ times gain in temporal resolution needs to be achieved, i.e., recovery of space-time volume $E$ needs to be done at a resolution of $M\times M\times N$. Let $S(x, y, t)$ denote the per-pixel shutter function of the camera within the integration time $(S(x, y, t) \in \{0,1\} )$. Then, the captured image $I(x, y)$ is 

$$I(x,y) = \sum_{t=1}^{N} S(s,y,t).E(x,y,t).$$

Rewriting the above equation in the matrix form as $\textbf{I} = \textbf{S}\textbf{E}$. $\textbf{I}$ which is the vector of observations has a size $M^{2}$ and $\textbf{E}$ which is the vector of unknowns has a size $N \times M^{2}$. To solve the under-determined system of equations above, techniques of compressed sensing are used. The sytem above can be solved faithfully if the signal $\textbf{E}$ has a sparse representation $\boldsymbol{\alpha}$ in a dictionary $\textbf{D}$:

$$\textbf{E} = \textbf{D}\boldsymbol{\alpha} = \alpha_{1}\textbf{D}_{1} + \cdots + \alpha_{k}\textbf{D}_{k}$$

where $\boldsymbol{\alpha} = \[\alpha_{1},\cdots,\alpha_{k}\]^{T}$ are the coefficients, and $\textbf{D}_1, \cdots , \textbf{D}_k$ are the elements in dictionary $\textbf{D}$. The coefficient vector $\boldsymbol{\alpha}$ is sparse. The over-complete dictionary $\textbf{D}$ is learned from a random collection of videos. The space-time volume $\textbf{E}$ is sampled with a coded exposure function $\textbf{S}$ and then projected along the time dimension, resulting in a coded exposure image $\textbf{I}$. 

An estimate of the coefficient vector $\hat{\boldsymbol{\alpha}}$ can be obtained by solving a standard problem in compressed sensing:

\begin{equation}
    \hat{\boldsymbol{\alpha}} = \arg \min_{\alpha} \lVert \alpha \rVert_{0} \text{ subject to } \lVert \textbf{S}\textbf{D}\boldsymbol{\alpha} - \textbf{I} \rVert_{2}^{2} \< \varepsilon
\end{equation}

The space-time volume is computed as $\hat{\textbf{E}} = \textbf{D}\hat{\boldsymbol{\alpha}}$. A pictorial representation of the approach containing three steps viz. coded sampling, dictionary learning and sparse reconstruction is given in figure below (taken directly from [Hitomi et al.](#1)).

<img src="/images/dictionary-learning-diagram.png" style="width:50%;">

<h2>Sampling Strategies</h2>

<h3>Continuous Bump</h3>
The continuous bump sampling strategy is a strategy that satisfies hardware constraints and is proposed by Liu et al. It satisfies the following constraints:

* <b>Binary shutter</b>: The sampling function $\textbf{S}$ takes values in $\{0,1 \}$. At any time $t$, a pixel is either collecting light (1-on) or not (0-off).
* <b>Single bump exposure</b>: Each pixel can have only one continuous “on” time (i.e., a single bump) during  a camera integration time. 
* <b>Fixed bump length</b>: The bump length is fixed for all pixels.

We implement this strategy as follows:
    1. For each pixel in the input video, the bump start time is selected uniformly randomly, i.e., the bump start time at any pixel can be $t$ with probability $1/N$ where $N$ is the temporal depth of $\textbf{E}$.
    2. Space time volume at a pixel is added when the shutter function is $1$. The coded image is constructed by averaging the above sum. 

<h3>Random Sampling</h3>
In this sampling strategy, the bump period may not be continuous. Fix a bump length $b$ and let the temporal depth be $N$, then this strategy is implemented as follows:
	1. For every point in an input video, i.e., a point described by $(x,y,t)$, the shutter function takes value 1 with probability $b/N$. 
    2. Space time volume at a pixel is added when the shutter function is $1$. The coded image is constructed by averaging the above sum. 
It might happen for a pixel location that the number of time instants where shutter function takes value 1 is not equal to the bump length.

<h3>Distributed Bump</h3>
Here, we constrain that for every pixel location, the number of time instants where shutter function takes value 1 is equal to the bump length. This is implemented as follows:
    1. Generate $b$ distinct random integers between $1$ and $N$.
    2. Space time volume at a pixel is added when the shutter function is $1$. The coded image is constructed by averaging the above sum.

<h2>Comments and Conclusions</h2>
For more information about OMP and K-SVD algorithm and details on our experimental setup and detailed results, I encourge the reader to see our report [here](/files/sp2019-rtasp.pdf). Finally, we have concluded that the method that we have experimented with in this project, can reconstruct the videos with high temporal resolution without compromising on spatial resolution, though some artifacts are visible, most likely because of smaller dictionary size that we have used. We observed the effect of changing different hyper-parameters. We also observed that distributed bump sampling produces the best results, but this has to be at the cost of increased hardware complexity.

<h2>References</h2>
<b id="#1">Video from a Single Coded Exposure Photograph using a Learned Over-Complete Dictionary</b><br>
Yasunobu Hitomi, Jinwei Gu, Mohit Gupta, Tomoo Mitsunaga, Shree K. Nayar (2011)<br>

<b id="#2">Efficient Space-Time Sampling with Pixelwise Coded Exposure for High-Speed Imaging</b><br>
Dengyu Liu, Jinwei Gu, Yasunobu Hitomi, Mohit Gupta, Tomoo Mitsunaga, Shree K. Nayar (2014)<br>


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