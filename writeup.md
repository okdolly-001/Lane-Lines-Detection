## Reflection

#### 1. Describe your pipeline.

My pipeline consisted of 5 steps. For preprocessing, I converted the image to grayscale and then do gaussian blurring.After that,I called openCV's Canny function to detect the edges, which are areas of an image where the colors change very quickly.In short,Canny edge detector is a strong gradient detector. 
[Here's the full algorithm:](https://web.stanford.edu/class/ee368/Handouts/Lectures/2014_Spring/Combined_Slides/11-Edge-Detection-Combined.pdf)
1.Smooth image with a Gaussian filter
2.Approximate gradient magnitude and angle
3.Apply nonmaxima suppression to gradient magnitude
4.Double thresholding to detect strong and weak edge pixels
5.Reject weak edge pixels not connected with strong edge pixels

I approximate the region of interests by feeding four coordinates, effectively two lines. For example, the bottom-left point is set as (0.1 ** width,height).

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by storing x and y coordinates of left lines and right lines separately. One thing to note is that my slope function is  slope = (y2 - y1) / (x2 - x1), because we know the y coordinates but not the x coordinates of the ROI. Only lines with slopes greater than 0.5 are considered, because the lines we want to draw must contain sharp edges.Right lines have positive slopes and left have negative.I used np.polyfit to get the coefficients of right and left lines respectively. Then I called np.poly1d to get a linear equation for left and right lines.Finally, our desired line can be found by plugging in the y values to the linear equation, which are height and (3/5) * height for both lines. Then I fed the two points describing left line and the two points for right line into the cv2.line function and voila, we have two separate, clean lines.



![labeled_solidYellowCurve.jpg][https://github.com/okdolly/Lane-Lines-Detection/blob/master/test_images_output/labeled_solidYellowCurve.jpg]


#### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


#### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
