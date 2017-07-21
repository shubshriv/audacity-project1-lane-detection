# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### 1. Pipeline 

My pipeline consisted of 5 steps. First, I created a mask to select while and yellow lanes. Creating a mask
seemed to be the easiest in HSL color space. I used the following masking values:
yellow_mask = (np.uint8([10, 50, 100]), np.uint8([50, 200, 255]))
white_mask = (np.uint8([0, 200, 0]), np.uint8([255, 255, 255]))

I then performed masking by doing bitwise and of the mask with the image in RGB space.

As the second step, I converted the color masked RGB image into a Grayscale image and applied gaussian blur with kernel size 3 to smooth out the noise.

Third step was to perform Canny edge detection which then returns an image with a logical value (true or false) for each pixel indicating whether or not that pixel returns the edge of a lane. If step one and two returns the result they were supposed to, then output of canny edge detection would be the edge of all the lane markings.

Fourth step in my pipeline was to create a region of interest which is simply a polygon mask for the relevant region of each frame. I defined this polygon to be a triangle at the lower part of the frame. This will filter out any other region that was wrongly detected as a lane in previous steps.

As the fifth step of my pipeline, I used Probabilistic Hough Transformation to obtain (x, y) of endpoints in each detected lanemarks in the frame. As a result of this transformation, we have start and end pixel index for each lane segments. However, it is important to determine and draw the lane boundary as a single line on the lane markings. I modified the draw_lines() function to perform this operation. I also renamed the function name to draw_lane_boundary()

To draw the lane boundary, I first separated all the lane endpoints available from the fifth step as either left lane or right lane based on the x value. After classifying all the lane segments as either left or right lane, I performed First Degree Polyfit on the lane segments to obtain slope and intercept for both left and right lane. I then regenerated the lane boundary from slope and intercept and wrote a function draw_point() to draw each regenerated point of custom thickness on the image.

### 2. Potential Shortcomings


One potential shortcoming in my code was that the lane boundary plotted was always a straight line even on the curvy road in challenge.mp4 because I used polyfit of degree one. 

The lane boundary drawn seemed to by noisy and flickery.


### 3. Suggest possible improvements to the pipeline

An improvement that needs to be done is to take care of the curves while plotting a single line for the lane boundary.

Another improvement that can be done is by filtering out the rapid change in slope of the lane in every consecutive frames. That could reduce the noise in the lane boundary.
