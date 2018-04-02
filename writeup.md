## Reflection

#### 1. Describe your pipeline.

My pipeline consists of 4 steps. 
1.For preprocessing, I converted the image to grayscale and then smoothed the image with Gaussian blurring.

2.After that,I called OpenCV's Canny function to detect edges, which are areas of an image where the colors change quickly.In short,Canny edge detector is a strong gradient detector. 

[Here's the full algorithm:](https://web.stanford.edu/class/ee368/Handouts/Lectures/2014_Spring/Combined_Slides/11-Edge-Detection-Combined.pdf)

  -Smooth image with a Gaussian filter

  -Approximate gradient magnitude and angle

  -Apply nonmaxima suppression to gradient magnitude

  -Double thresholding to detect strong and weak edge pixels

  -Reject weak edge pixels not connected with strong edge pixels


3.I approximated the region of interests by feeding four coordinates, effectively two lines. For example, the bottom-left point is set as (0.1 ** width,height).


4.In order to draw a single line on the left and right lanes, I modified the draw_lines() function by storing x and y coordinates of left lines and right lines separately. One thing to note is that my slope function is  slope = (y2 - y1) / (x2 - x1), because we know the y coordinates but not the x coordinates of the ROI. Only lines with slopes greater than 0.5 are considered, because the lines we want to draw must contain sharp edges.Right lines have positive slopes and left have negative.I used np.polyfit to get the coefficients of right and left lines respectively. Then I called np.poly1d to get a linear equation for left and right lines.Finally, our desired line can be found by plugging in the y values to the linear equation, which are height and (3/5) * height for both lines. Then I fed the two points describing left line and the two points for right line into the cv2.line function and voila, we have two separate, clean lines.



![alt text](https://github.com/okdolly/Lane-Lines-Detection/blob/master/test_images_output/labeled_solidYellowCurve.jpg)


#### 2. Identify potential shortcomings with your current pipeline


I hardcoded the region of interest by visually inspecting the image and deciding that the far point of the lane line is around 3/5 of the height. However, it is not always the case. Also, lane lines are likely parabolic when we make a sharp turn and as a result, the vanishing point of the lane line might be best described by a third-order polynomial.

Another shortcoming is that I should have used both yellow and white masks to get the region of interest,because yellow is a major part of the lane line.

#### 3. Suggest possible improvements to your pipeline

A possible improvement would be to set better parameters for ROI. For detecting radius of curvature, YOLO and Faster R-CNN are good choices.
