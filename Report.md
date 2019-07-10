This report come as part of the Udacity Nanodegree on Self Driving Car Engineering.

### Introduction
  In this project, the objective is to use the OpenCV library to perform track line recognition. We will start by testing our algorithm on images and then directly on videos.


### Description
  In this project, the main part has already be seen during the course. The expectation here was to assemble the whole in an algorithm that will be able to process directly video input. In order to do so, we had to follow a certain number of steps which are the following:
  
* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.
  
All the code for each step of this algorithm is contained in the `P2_Notebook`. We'll go through every step one by one.

1.Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.

  The code for this step was given in the example file and is by consequent in the first cell of the nootebook. In this part, we take 20 images with different angulars of the same white and black chessboard and thanks to the function cv2.findChessboardCorners() from the openCV library we can find the straight line and the corners of the chessboard.

2. Apply a distortion correction to raw images.

  For this step, we use the cv2.calibrateCamera() function in order to find the coefficients of distortion from our camera by using in input, the ret and corners that we defined in the last step.
  Once we've got our coefficients, we can undistort our images. In order to avoid to run this code everytime, we'll save the coefficient in a file named `wide_dist_pickle.p`.
  
  Here you can see a picture undistored of the chessboard :
  ![](output_images/Undistorted_Image.png)

3. Use color transforms, gradients, etc., to create a thresholded binary image.

  Now, in this part, we will try to create a binary image on which we can easily distinct the lane lines. In order to do so, we'll use 3 different filters:
  
  - The first one will detect the edges of the images and we'll try to tun it in order to have the lane lines edge in view most of the time.
  - The second one will be the absolute derivative on x axis of the light in the image. We are able to do this after having convert the RGB image into the HLS space(Hue, Saturation, Lightness).
  - Finally, the last one will be a threshole on the saturation in the image.
  
By adding, this three filters together, we are able to obtain an image like this one:
  ![](output_images/img_test6_after_pipeline.png)

4. Apply a perspective transform to rectify binary image ("birds-eye view").

First, in this part,we need an image where the road contained straight lines, then we get two points on each lines and finally we can have a top-down perspective with the function birdview() by determining four points making a rectangle.
  In order to have my points, i decided to use the algorithm of the last project which allow us to have coordinates of points matching perfectly the lines for images with not too much noise. So with this algorithm, I define the points on the lane lines in the extremity of the purple lines and the points I need in the extremity of the green lines:
![](output_images/straight_lines2_for_perspectives.png) 

Then, after using the function birdview(), we can see the road like this:
![](output_images/Undistorted_and_Warped_Image.png)  

5. Detect lane pixels and fit to find the lane boundary.

In this part, we'll detect the lane lines pixel which is easier since we've applied our filters and we'll try to calculate the scd order polynomial curve that fit the better the lanes we found. In order to do so, we will cut the image along the y axis in nwindows blocs and then we will search in this bloc for the lanes lines in order to have many differents points from the lines before interpolating our polynomial curve. Here is the result:
![](output_images/detection_of_the_lanes.png) 

Then if we are processing a video, the curves on two following images will be approximately the same. So, in order to gain time, we'll juste search in the nearby area of the lane lines of the last image and we'll obtain something like that:
![](output_images/search_around_the_lanes.png) 

6. Determine the curvature of the lane and vehicle position with respect to center.
 Here, we define the value in meters for one pixel on x and y axes and then we calculate the average curvature of the two lines at the bottom of the image with the function mesure_curvature(). We also mesure the distance of the car from the center of the road. To do so, we suppose that the camera is in the middle of the car even if it seems to have an offset of 20 or 30cm. Now we can display this two figures on the video while processing it.


7. Warp the detected lane boundaries back onto the original image.
8. Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

Step 7 and 8 are made in the same code cell, and the result once we've applied all the function from the precedent steps is that:

![](output_images/green_Area.png) 
  
  
  Finally, we use all those function to process each images of the videos and we obtain something great for the project video and.. something desastreous for the two challenge ones. Though, there are still many variables that we can tun or modifications in each steps that we can makes. We'll see that in the next part.  

### Reflexion

  Now that the algorithm effectively works for simple road tracks, we have to improve it in order to make it more robust and to be able to detect the lane lines much more easily.

  I think there at least 2 main points on which we could improve the efficience of the algorithm:

- At first, of course we could still try to optimize the variables of the image manipulation (pipeline function). In order to do so, as explain in the Udacity course, we could create a simple interface to modify each value in real time in order to gain time.
- Then we should try to improve the algorithm for lane finding. For example, we could train a simple neural network to recognize the lanes after the image have been processing. Once this one will be well trained, it should also require less processor to run.


Finally, it would be usefull to try to reduce the complexity of the algorithm in order to speed the time needed to process one image. In did, this algorithm is suppose to work in live and by consequent need to be able to process an image in at least less than 40ms. Of course, it depends of the processor but we still will have many other calcul to do sideway.
