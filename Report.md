This report come as part of the Udacity Nanodegree on Self Driving Car Engineering.

### Introduction
  In this project, the objective is to use the OpenCV library to perform track line recognition. We will start by testing our algorithm on images and then directly on videos.


### Description
  In this project, the main part has already be given during the course. The most important things we had to do was, first, to modify the variables in order to have a good enderstanding of the lane line and, secondly, modify the draw_line function in order to properly draw the estimation of the lane lines.
  At first I tried to modify the variable from the ones that were given during the course but I didn't succeed in improving the output so I stayed with the Udacity ones.
  Then for the draw_line function, I choose to simply delete all the slope that were too close from the horizontal (which means slopes between -0.5 and 0.5) then I separate them in two parts according to the side they were in the image. After that, by taking the mean of each lines, I was able to find an estimation of the slope of each lane and an estimation of a point in that line which lead me to draw two lines approximately on the left and right lanes.

### Reflexion
  Curently, the algorithm is good enough to recognize left and right lanes on verry simple environment. Though, it is still not efficient enough for the optional challenge video given at the end of the `P1.ypnb` Notebook and it is a little bit noisy.
  
  I think there at least 3 points on which we could improve the efficience of the algorithm:
  
- At first, of course we could still try to optimize the variables of the image manipulation (kernel_size, low_threshold, high_threshold, rho, min_line_length, etc). In order to do so, as explain in the Udacity course, we could create a simple interface to modify each value in real time in order to gain time.
- Then, in the algorithm, I choose to separate the lines from the left side to the ones from the right side. To improve that at first, we should maybe try to have a better separation between the lines for the left lane and those for the right lane. In fact, now if the road turn too much, the algorithm won't be able to have a good understanding of the images. Then we could delete the lines with absurd slopes in comparison to the slope mean of all the lines of the left or right lane.
- Finally, we could add a time continuity (taking into account the previous position of the lanes) to cancel the noise on the position of our lane lines and avoid anny absurdity which could occur time to time.
