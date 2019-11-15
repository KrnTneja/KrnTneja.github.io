---
title:  "Computer Graphics: Music Box Modelling, Rendering and Animation"
date:   2018-11-10
permalink: /posts/2018/graphics-music-box
tags:
  - course-project
  - animation
  - rendering
  - graphics-modelling
---

I took the course <i>Computer Graphics</i> in Autumn 2018 offered by [Prof. Parag Chaudhuri](https://www.cse.iitb.ac.in/~paragc/) at IIT Bombay. This is a blog post about the work I did for the course project with Pranav Kulkarni on modelling a music box with a humanoid figure and a giraffe in a realistic room with lighting and texture and rendering the animation of dancing characters with camera movement on user-specified Bezier curve. The project was implemented in OpenGL. An example of animation [video](https://www.youtube.com/watch?v=rc9bLdhiwLg) generated from our interface can be be seen on YouTube. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/rc9bLdhiwLg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Easiest part was modelling the box, we simply used two triangles to create a face, six faces to create cuboids and seven cuboids were put together to create the box along with a partition in the middle. In the hierarhial model, the lid of the box is the child of rest of the body. A key callback function is set to open and close the lid by incrementing/decrementing the angle of lid with box along appropriate axis	.

To model the human and giraffe, we first make a cylinder. The 2 circular faces of cylinder are made up 100 sectors each where these sectors are approximated with traingles. Further, the cylinderical surface is made up of 100 rectangles each having a common edge with both circular faces. These rectangles are themselves mafe up of t 2 traiangles each. In total, we used 400 triangles to approximate our cylinder. In other words, it's a 100-sided prism. 

Humanoid has root of hierarhy at its hip which is a general setting. Torso is child of hip and shoulder is the child of torso. Legs are attached at torso and arms at shoulder. A head and neck is attached at shoulder. We first set the default transformations to all the body parts to make the human figure and then set the constraints on motion of different parts for body to ensure that final output after user adjusts the figure still looks realistic. Other small details like hair, eyes, lips are also made by adding cylinders of different sizes and orientation. There is no control to move them in our model.

Giraffe also has a very similar hierarhial model where torso is at the root. Four legs, a tail and neck are attached to torso and a head is attached to the other end of long neck. Other fixed details are added just like we added them for humanoid. 

The root of both humanoid and giraffe is set as the child of box and we limit the degrees of freedom to contrain motion in upwards/downwards direction only. A callback function is set for different key sequences to first select the body part and then six keys are set to increase or decrease different angles on the particular joint. 

We also created a room with textured wall, sofas, chairs, a lamp, wall lamps, curtains and a door. The music box is kept on the table. Our interface allows the user to move around the room and capture different points in sequence. Then we render an animation by moving the camera along the Bezier curve formed by these captured points. All the frames are for music box are captured by a vector of angles of all the degrees of freedom in present in the music box. For the animation, we just interpolate frames between these vectors to make animation.   

To generate the video shown above, we go around the room once using Bezier curve and then see the lights go up at different levels when music box opens up using interpolation between a sequence of frames. Our talented humanoid and giraffe give their best performance on the amazing music cover of [Despacito by Peter Bence](https://www.youtube.com/watch?v=GmtTDvNcXcU) and take a innocent exit back into the box. Lights out! 

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