---
title:  "Batch Arm Pulls for Stochastic Multi-Armed Bandits"
date:   2019-11-04
permalink: /posts/2019/batch-arm-pulls-mabs
tags:
  - reinforcement-learning
  - multi-armed-bandits
  - concentration-inequalities
  - course-project
---

I took the course <i>Advanced Concentration Inequalities</i> in Autumn 2019 offered by [Prof. Sharayu Moharir](https://www.ee.iitb.ac.in/web/people/faculty/home/sharayum) at IIT Bombay. This is a blog post about the work I did for the course project with Shashwat Shukla and Sucheta Ravikanti on designing regret minimization algorithms for the problem of batch pulls in stochastic multi-armed bandits. 

In the batch setting, multiple arms are pulled at the same time, and the rewards from these pulls are returned together at the end of the round. Thus, we must jointly pick $K$ arms to explore and exploit in this setting, and it was this richer setting that was of interest to us. Applications of this model include parallel computing, advertisement models, mining exploration etc. We designed algorithms both for the setting of minimizing simple regret (which corresponds to pure exploration) as well as for cumulative regret (which balances exploration and exploitation).

For the case of simple regret, we proposed an algorithm that perturbs the current estimates of the mean arm rewards and then picks the top $K$ perturbed arms in each round. Our algorithm has a lower sample complexity than the previous state-of-the-art algorithm by a factor of $N/K$, where $N$ is the total number of arms, while still returning an answer after the same number of rounds. This is a massive reduction in sample complexity (say $N=1000$ and $K=10$), and we verified the correctness of our algorithm via extensive simulations. Our proposed algorithm is essentially a randomized form of UCB and it is thus very interesting to note that a UCB-like algorithm is also working so well for the pure exploration problem of reporting the top-K arms. The beautiful understanding that emerges from our study is that in a finite time setting, this UCB-like algorithm balances safe exploration with ambitious exploration, and is thus a useful and very different viewpoint from the typical exploration-exploitation tradeoff. Due to the limited time-horizon available, this tradeoff is benificial and allows us to significantly outperform previously proposed uniform sampling methods. Thus our algorithm is strongly reminiscent of the lil’UCB algorithm proposed for the $K=1$ setting.

We also designed novel algorithms for minimizing cumulative regret in the setting of batch pulls for Gaussian Process Multi-Armed Bandits. We were interested in this setting because with prior information about correlated values of ‘nearby’ arms, finding the optimal set of arms to pull is a richer problem. Gaussian processes allow us to formalize our prior beliefs about these correlations between mean rewards values of arms while also allowing us to compute the posterior to make principled online decisions. We designed batch versions of GP-UCB that go beyond the naive choice of confidence bounds for the rewards from $K$-arm subsets. We did this by using the simple observation that the variance of the sum of correlated random variables, mean arm rewards in this case, is not equal to the sum of their variances. Another interesting observation we made is that the update equation for the covariance matrix of the Gaussian Process only uses the locations of sampled arms. We used this to design a new algorithm that uses ‘virtual’ arm pulls within a batch to pick the $K$ arms in a way that is better at balancing exploration and exploitation in this interesting setting. We also compared these batch GP-UCB algorithms to our proposed batch GP-TS algorithm.

The slides from our project presentation can be found [here](/files/a2019-batch-pulls-mabs.pdf).


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