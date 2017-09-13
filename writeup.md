
# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/lanelines_edges.png 
[image2]: ./examples/lanelines.png 


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I performed Gaussian blur filter processing and applied Canny edge detection algorithm to the images as preprocessing steps.

After these, lane lines are detected by using Hough transform technique.

In this step, the detected edges of lane lines are converted to two thick lines for both sides of the lanes.
This is performed by modifying the `draw_lines()` function 
Finally I overlayed these lines on the original image by `weighted_img()`.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function as following:
 - I grouped lines into left or right lanes by checking gradient's polarity in `filter_lines()`.
 - For each groups of lines, I fitted one line to them and extrapolated it in `extrapolate_lines`.
 - Then I get two thick red lines over lanes as an image in `draw_extrapolated_lines`.


The following images show how the above pipeline works: 

This image demonstrates edged lane lines after Hough transform, which are actually not shown in my resulting image.
![alt text][image1] 

This is my final image obtained after all steps above explained.
![alt text][image2] 



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the car runs on a sharp curve because only straight lane lines could be detected by my algorithm. So in this case only short lines close to the car would be drawn.

Another potential shortcoming is the hard coding of the margin(`XMARGIN` in `extrapolate_lines`) between two edge points of the crossing lane lines. `XMAX` should also be adjusted to the image width.

The non-robustness of fixed Hough transform parameters is also an issue. Actually, in the two videos, the overlayed lines sometimes stick out to the other side due to inappropriate detection of lines, I guess.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to change the constants of the extrapolation of lines dynamically based on the actual size of the current image and the space between lane lines.

Another potential improvement could be to use second order polynomial fitting so as to detect curved lane lines.
