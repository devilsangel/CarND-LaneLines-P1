# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteCurve.jpg "whiteInput"
[image2]: ./test_images/solidYellowCurve.jpg "yellowInput"
[image3]: ./test_images_output/blurred_image/solidYellowCurve.jpg "whiteblur"
[image4]: ./test_images_output/blurred_image/solidYellowCurve.jpg "yellowblur"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
Considering the following examples
1. Initially I had done the first step by converting the image to grayscale followed by gaussian blurring, but attempting the challenge video and some research gave me a better approach which is converting the image to hsv and binarizing. I will explain my reason for the same later in this file.
![Input][image1]
2. Running canny edge detection on the binary image was the next step. I chose thresholds 50,150(in the ration 1:3).
3. Then I cropped out a trapezoidal area as the region of interest from the edge detected image.
4. This was followed by the hough lines algorithm on the edge detected masked image,which returned a bunch of lines.
5. As the next step I improved the draw_lines() function. I split the lines into right and left based on their slopes while filtering out the horizontal lines. Then I used linear regression on the points in each group to obtain a best fit line, and then drew a line from the top-most point in the group to the base of the image using the equation of the best fit line obtained.

6. The above pipeline worked pretty well on the test images and the first 2 videos. I had to make some changes in order for the pipeline to work for the challenge video. One of those changes is the one i mentioned in step 1. 
    i) Ofcourse the main problem was that the challenge video was of another dimension, which means my region of interest with fixed pixel points failed. As a solution I specified my region of interest points as fractions of the height and width of the input image.
    ii) The second problem I observed was that random lines(skid lines I think) on the road(within the lane) also would get detected and distort the lines. As a soution I made the change mention in step 1, instead of grayscale followed by gaussian blur I used HSV followed by binarization using yellow and white colors. This worked better because the lane lines we need to detect here are yellow or white while most of the noise lines are not.


### 2. Identify potential shortcomings with your current pipeline


I observed the following shortcomings:
1. Any distinguishable lines in the region of interest will distort the lane detection. For example, if a car is moving right in front of our self driving vehicle, the edges of the car will distort the detection.
2. The region of interest is a hard-coded region of interest, which makes this pipeline unfit for use if the lane is not inside that region. Also this might be a problem on curvy roads(mountain roads).
3. Another shortcoming I observed is that the detection gets distorted in low light.I observed this while running the challenge video,the lines would get distorted when it passes through shadow regions(tree shadows).


### 3. Suggest possible improvements to your pipeline

I will suggest improvements as solutions to the above mentioned shortcomings:
1. Using feature detection using neural networks, instead of edge detection might help us remove unnecessary parts of the image better.
2. Again feature detection using neural networks might allow us to detect the region of interest based on the input. Another solution might be specifing a tight set of rules for ruling out noise lines, instead of specifying a region of interest.
3. 
