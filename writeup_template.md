# **Finding Lane Lines on the Road** 

## Writeup Template

## PRANAV BAROT 20616032


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[i1]: i1.png "Original"
[i2]: i2.png "HSL"
[i3]: i3.png "Grayscale"
[i4]: i4.png "Edges"
[i5]: i5.png "ROI"
[i6]: i6.png "Hough Lines"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of some unique steps prior to the prescribed guidelines, to allow for better line detection and extrapolaton. 
They are as follows:

1. Convert image to HSL
2. Distinguish yellow and white from the HSL images
3. Apply grayscale
4. Apply Gaussian Blur
5. Use canny edge detection
6. Find ROI, discard all other lines outside this region
7. Use Hough transform on this region, trace lines
8. Separate left and right lanes, extrapolate to create two smooth, separate lines


Each step is incrementally shown here.

![text][i1]

Example 1:

![alt text][i2]

Example 2: Find white and yellow in the images using thresholding (shown in notebook). BITWISE AND this result with the image 

![alt text][i3]

Example 3: Test out edge detection filtering

![alt text][i4]

Example 4: Apply trapezoid ROI to filtered image, get just the lanes in front of the car

![alt text][i5]

Example 5: Get Hough transform lines. Plot them using cv2

![alt text][i6]


I attempted to write my own Hough transform function, and the results appeared close to being correct but were not exactly what was necessary.
I relied on the openCV implementation instead. 

For the draw_lines() function, I first chose to separate the detected lines into two categories: left lanes and right lanes.

I used the slopes of the lines and where the lines resided to do this 
 - (based on the coordinate system, left lanes must have a negative slope and live in the left half of the image, right lanes are the opposite)

Then, I simply had a set of points that would typically describe one straight line for each half of the image.

I used a stats library to regress the equation of this line given the points for the left and right lines, and simply drew the straight line equation on the image
throughout. The same process was applied for every frame of the test video inputs. 


### 2. Identify potential shortcomings with your current pipeline


1. A key shortcoming could be the presence of curved lines. A straight line equation may not be able to handle this, and will look extremely inaccurate
on roads with curved lanes.

2. Another issue is the lighting: all test inputs work in well lit daytime environments, but poor weather or nighttime images may be completely different,
especially considering the quality of the HSL filtering to identify white/yellow lane lines.

3. There is a noticed jitter in the drawn lines on certain frames in the video (where lines seem to jump around a little bit). This may be due to the nature of the extracted points not being fully in line from frame to frame


### 3. Suggest possible improvements to your pipeline

1. One improvement is to implement curve fitting for these lines. Choose to use a curve rather than a straight line when the std_error of the computed straight line
gets very high (this must indicate there is a curve) then simply draw the curve on the current frame rather than the straight line. This way lane detection takes care of both straight lanes and curved lanes

2. Another improvement is to use a "mean" line that follows the lane and only slightly deviates from frame to frame as new points are detected in subsequent frames. This translates to having a dynamic list of x and y coordinates that are dequeued from the front once a new coordinate is appended to the end. The line is simply computed on every frame with the coordinates present in the list at the given timestep.
