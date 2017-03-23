**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/output/solidWhiteCurve.jpg "WhiteCurve"
[image2]: ./test_images/output/solidWhiteRight.jpg "WhiteRight"
[image3]: ./test_images/output/solidYellowCurve.jpg "YellowCurve"
[image4]: ./test_images/output/solidYellowCurve2.jpg "YellowCurve2"
[image5]: ./test_images/output/solidYellowLeft.jpg "YellowLeft"
[image6]: ./test_images/output/whiteCarLaneSwitch.jpg "CarLaneSwitch.jpg"

---

**Reflection**

**1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The first step in the pipeline was to define all of the input variables needed for the various functions called during the process. By putting them in a single clearly commented block, it was easy to iterate on quick changes to the functionality of the algorithm.

The actual image processing script involved 6 functions/steps:
1. Convert to grayscale using the *grayscale()* function
2. Apply *gaussian_blur()*
3. Find edges using Canny edge detection with *canny()*
4. Apply an image mask (trapezoidal shape) using *region_of_interest()*
5. Apply *hough_lines()* function to edges found in step 3 that are within the area of the image mask defined in step 4.
6. Use *weighted_image()* to add the lines to the original image. 

The *hough_lines()* function calls a *draw_lines()* script. The default version simply draws the lines as provided by the Hough transform. This creates a number of small individual lines. In order to determine a single line for each lane, the *draw_lines()* function was modified and renamed to *draw_lines2()*. The new *draw_lines2()* function sorts each line according to slope. A range of slopes is defined for the left lane, and a corresponding range of slopes is defined for the right lane. If a particular line has a slope within either criterion, its [(x1,y1),(x2,y2)] location is added to a running sum and a counter is incremented. After looping through all of the lines, an average is calculated for each x and y coordinate (for each lane). From this, a slope and y-intercept is found for each lane. Given a minimum and maximum y-coordinate, the corresponding x-coordinate is found for each point (2 for each lane, 4 points total). These points are then plotted to show a single left and single right lane. 

Below are sample images showing the lanes being displayed successfully:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

**2. Identify potential shortcomings with your current pipeline

**Mask**

The pipeline works well as long as the lanes are within a particular location in the frame. If they are outside of the mask it will not work well. 

**Edges that are not part of the lane lines**

Also, if the edge detection finds similarly sloped lines within the region of the mask that are not part of the lane, due to the averaging the lane is drawn in a very poor location. The challenge case causes this problem. 


**3. Suggest possible improvements to your pipeline

**Mask**

It may be possible to scale or adjust the location of the mask based on features in the frame. This would help with lane changes, curved roads, and uphill and downhill. 

**Edges that are not part of the lane lines**

The problem with the averaging is that any mistakes must be weeded out. It would be possible to weight the positions of each of the lines based on how close they are to the final average, with a higher weighting for being closer to average. This might help reduce the effect of outliers and increase the accuracy of the averaged lane. 

Another possibility would be to filter out those outliers so that they are not included in the average. Since they are already being filtered by slope, this would be to be done by position. It is possible that additional slope tuning may improve performance as well. 

