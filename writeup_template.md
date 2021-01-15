# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of some unique steps prior to the prescribed guidelines, to allow for a better line detection and extrapolaton. 
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

![alt text][i1]

Step 1:

![alt text][i2]




### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
