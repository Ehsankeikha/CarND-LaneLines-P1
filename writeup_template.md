
---




[//]: # (Image References)

[image1]: ./output_image/testimg_1.png "Grayscale"
[image2]: ./output_image/grayimg.png "Grayscale"
[image3]: ./output_image/blurimg.png "Grayscale"
[image4]: ./output_image/edgeimg.png "Grayscale"
[image5]: ./output_image/regionimg.png "Grayscale"
[image6]: ./output_image/piplineimg.png "Grayscale"

## Reflection

### 1. Describe your pipeline. 

In this part, I will cover in detail the different steps needed to create my pipeline, which will enable me to identify and classify lane lines. My pipeline consisted of 5 steps:


0- load the image:

![alt text][image1]
  
1- Convert the images to grayscale

![alt text][image2]

2- Applying a Gaussian blur to the image

![alt text][image3]

2- Applying Canny edge detection

![alt text][image4]

4- Select region of interest and mask other areas of the image

![alt text][image5]

5- Running the Hough transform to identify lines


 

#### 6 -Converting lines into straight lines:

In order to draw a single line on the left and right lanes, I modified the function draw_lines(). This function works as follows:

* Calculated slope and center of each line. Then based on the slope, sort it into right or left lane line

* To Smoothing the result remove the outlier lines based on slope and location of the lines.

* use the memory of previous frames to improve the lane detection. Since a video is a sequence of frames. We can therefore use the information from previous frames to smoothen the lines that we trace on the road and take corrective steps if at frame t our computed lines differ disproportionately from the mean of line slopes and intercepts we computed from frames [0, t-1]. We therefore need to impart the concept of memory into our pipeline. I use a standard Python global variable to store the last N (I set it at 60 for now) computed line coefficients.

* The last step is to polynomial to the all points. Because of the curvature of the lanes a straight line formed by a simple polynomial of degree 1 (i.e. y = Ax¹ + b) would not be enough.

7- Plotting the lines on top of the image



## 2. Identify potential shortcomings with your current pipeline

* Slope conditions used for detecting right and left lanes do not work in case of a curve in the road.

* In low-contrast situations, the line identification pipeline may not work well.

* At intersections with sharp angles, the code cannot draw over the entire lane marking.

* The region of interest in Image masking is static, hence it can only work in specific scenarios


## 3. Suggest possible improvements to your pipeline

* Make the image mask selection dynamic, so that it could work in different scenarios modify the slope conditions, so that they work with curve in the road.

* The optimal parameters for Gaussian smoothing, the Hough transform, moving average, etc. might be different based on the input video. A more advanced pipeline could self-calibrate based on the observed quality of lane identification. Some ML could be used here.

* Calculate the weighted average of line coefficients in the  MemoryLaneDetector, giving a higher weight to more recent coefficients as they belong to more recent frames;

* Another improvement would be removing global variables in favor passing average values in a functional programming manner.
