# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of the following standard 5 steps as explained in the tutorials:
     1. Transform image to grayscale and apply Gaussian blur to reduce noise.
     2. Use the **Canny transform** to detect edges in the grayscale image. The values 50 and 150 were used for  the lower and high thresholds.
     3. Cut out a region of interest and apply as mask to the image from the canny edge detection.
     4. Use **Hough's transform** to detect the lane lines on the image (see code for parameters).
     5. Use the *weighted_img* function to color and display the detected lines on a copy 	of the original image.

     

In other to completed the final part of the project, I choose to write a new function called ***draw_avg_lines*** due to challenges which shall be explained below.
The ***draw_avg_lines*** function consists of the following steps.

1. Using the function *numpy.polyfit* I was able to extract the slopes and intercepts of all lines to be plotted.
2. Using the slopes I was able to distinguish the left-sided from the right-sided lines. 
   Given a slope is defined as *slope = (y2 - y1)/(x2 - x1)*, picking a random point for each line on the test image and applying to the above equation gives us a negative slope for the left line and positive slope for the right line.
3.  Once an array of slopes and interfaces has been collected for all lines, the average of each parameters can be computed for both lines using the *numpy.average* function.
4.  Using the average slope and average intercept we can calculate the x1, y1, x2 and y2 coordinates for each line using the cartesian equation of  a line and some intuition. 
    Given we know the average lines will originate from the bottom of the image and end at the region defined by us (0.6 of image height), it is  therefore safe to say the coordinates of the average lines will look as follows
		*(x1, y1 = image.shape[0])* and *(x2, y2 = (0.6 x image.shape[0]))*.
	x1 and x2 can be calculated using the cartesian equation of line *y = mx + b*


The deduced average lines are then plotted using the function *cv2.line*.

The function ***draw_avg_lines*** is called from within the Hough lines function as needed.



##### Problems

Trying to use the draw_lines function to the plot average lines produced the compile error  "*TypeError: 'numpy.int32' object is not iterable"* since this function expects a 2-D array.

The parameters for the Hough transforms gotten from testing on the test image were almost irrelevant for the video frames i.e. no advantaged gained working on the test image first.

For some reason some lines passed on by the Hough transform are invalid, causing the *draw_avg_line* function to return Nan when trying to compute the average this lines. Unfortunately I ran out of time to try to figure the origin of this problem, so for now this frames are simply ignored (see line 76 under Helper functions).

If you'd like to include images to show how the pipeline works, here is how to include an image: 




### 2. Identify potential shortcomings with your current pipeline

As seen when running the pipeline on the challenge.mp4 video, the pipeline is horrible on curved roads.


### 3. Suggest possible improvements to your pipeline

Due to time constraints I have not been able to research on possible improvement. However intuitively I will say, reducing the region of interest might help improve the results on the challenge.mp4 video.
