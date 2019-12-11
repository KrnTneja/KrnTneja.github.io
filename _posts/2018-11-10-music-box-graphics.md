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

I took the course <i>Computer Graphics</i> in Autumn 2018 offered by [Prof. Parag Chaudhuri](https://www.cse.iitb.ac.in/~paragc/) at IIT Bombay. This is a blog post about the work I did for the course project with Pranav Kulkarni on modeling a music box with a humanoid figure and a giraffe in a realistic room with lighting and texture and rendering the animation of dancing characters with camera movement on a user-specified Bezier curve. The project was implemented in OpenGL. An example of animation [video](https://www.youtube.com/watch?v=rc9bLdhiwLg) generated from our interface can be be seen on YouTube. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/rc9bLdhiwLg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
The easiest part was modeling the box, we simply used two triangles to create a face, six faces to create cuboids and seven cuboids were put together to create the box along with a partition in the middle. In the hierarchical model, the lid of the box is the child of the rest of the body. A key callback function is set to open and close the lid by incrementing/decrementing the angle of the lid with the box along the appropriate axis.

To model the human and giraffe, we first make a cylinder. The 2 circular faces of the cylinder are made up of 100 sectors each where these sectors are approximated with triangles. Further, the cylindrical surface is made up of 100 rectangles each having a common edge with both circular faces. These rectangles are themselves made up of 2 triangles each. In total, we used 400 triangles to approximate our cylinder. In other words, it's a 100-sided prism. 

Humanoid has the root of hierarchy at its hip which is a general setting. The torso is the child of the hip and the shoulder is the child of the torso. Legs are attached at torso and arms at the shoulder. A head and neck are attached at the shoulder. We first set the default transformations to all the body parts to make the human figure and then set the constraints on the motion of different parts for the body to ensure that final output after the user adjusts the figure still looks realistic. Other small details like hair, eyes, lips are also made by adding cylinders of different sizes and orientations. There is no control to move them in our model.

Giraffe also has a very similar hierarchical model where the torso is at the root. Four legs, a tail, and neck are attached to the torso and a head is attached to the other end of the long neck. Other fixed details are added just like we added them for humanoid. 

The root of both humanoid and giraffe is set as the child of the box and we limit the degrees of freedom to constrain motion in upwards/downwards direction only. A callback function is set for different key sequences to first select the body part and then six keys are set to increase or decrease different angles on the particular joint. 

We also created a room with a textured wall, sofas, chairs, a lamp, wall lamps, curtains, and a door. The music box is kept on the table. Our interface allows the user to move around the room and capture different points in sequence. Then we render an animation by moving the camera along the Bezier curve formed by these captured points. All the frames for the music box are captured by a vector of angles of all the degrees of freedom present in the music box. For the animation, we just interpolate frames between these vectors to make animation.   

To generate the video shown above, we go around the room once using a Bezier curve and then see the lights go up at different levels when the music box opens up using interpolation between a sequence of frames. Our talented humanoid and giraffe give their best performance on the amazing music cover of [Despacito by Peter Bence](https://www.youtube.com/watch?v=GmtTDvNcXcU) and take an innocent exit back into the box. Lights out! 