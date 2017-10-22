# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

1. I converted the images to grayscale and applied Gussian smoothing. 
2. I used Canny edge detection to detect the edges of all objects. 
3. I defined a four sided polygon to mask the image, only leave the area in front of the car, where the lanes are.
4. I used Hough transformation to find the lane lines, and use OpenCV to draw the lines. The output is an image with only the lane lines. 
5. I used OpenCV to overlay the original image with the lane image from step 4

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by extrapolation:
1. for each line detected by Hough transformation, find the slope
2. Decide if a line is left lane or right lane in such: if slope is greater than a threshold, it's the right lane. 
If the slope is less than the negative value of the threshold, it's the left lane. The reason why I used a very small value as the threshold, 
instead of 0, is to eliminate horizontal lines. Add the two points of the line to the list of left points, and right points. 
3. Use *numpy.polyfit* to fit the left and right lane points seperately, in order to find the slope and intercept of the lanes. 
4. Based on the area of interest to decide the top and bottom y coordinate of the lanes, 
5. Use slope and the intercept of the lane to calculate the x coordinates corresponding to the top and bottom y coodinate.
6. Use *cv2.line* to draw line for left and right lane, providing the top and bottom x and y coordinates. 

The following is the images and videos that draw solid line for the lanes, after apply the improved version of *draw_line* function:

[Images with lane drawn in solid lines](https://github.com/melindadevins/CarND-LaneLines-P1/tree/master/test_images_improved_output)

[Videos with lane drawn in solid lines](https://github.com/melindadevins/CarND-LaneLines-P1/tree/master/test_videos_improved_output)


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be the human factor: after use Canny edge detection to find edges of all objects in the image, 
I eyeballed the image and decided which area in the image contained the lanes. 
I then used a polygon to mask off the area that do not contain the lanes. 
I doubt it is feasible to pre-determine the area where the lanes are when the rubber hits the road. 

Another shortcoming could be the way how I extrapolate the lines. I assumes the lanes were straight lines, 
and used one degree polyfit to fit the slope and intercept of the line. It will not work well when the lanes are curved. 

Third problem was that the changing of light on the road was mistakenly draw as lane in some frames of the optional challenge video. 

### 3. Suggest possible improvements to your pipeline

A possible improvement would be using deep learning object detection to seperate the lanes from other objects in Canny edge detection's output image, 
instead of rely on human's eyeball to decide the area where the lanes are and mask off other areas.

Another potential improvement could be when draw the lines for the lanes, extrapolate the curve lines, instead of assuming straight line. 

To eliminate the light reflection on the road, perhaps one could find a better gray scale. 