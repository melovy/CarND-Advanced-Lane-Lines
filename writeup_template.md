## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/examples/test1_original.jpg "Original"
[image2]: ./output_images/examples/test1_undistort.jpg "Undistorted"
[image3]: ./output_images/examples/test1_binary.jpg "Binary Example"
[image4]: ./output_images/examples/test1_warped.jpg "Warp Example"
[image5]: ./output_images/examples/test1_final.jpg "Output"
[image6]: ./output_images/chessboard/calibration2_original.jpg "Chessboard Original"
[image7]: ./output_images/chessboard/calibration2_corner.jpg "Chessboard Corner"
[image8]: ./output_images/chessboard/calibration2_undistort.jpg "Chessboard Undistort"
[image9]: ./output_images/examples/perspective_transform.jpg "Perspective Transform"
[image10]: ./output_images/examples/find_lanes1.jpg "Sliding Window Search"
[image11]: ./output_images/examples/find_lanes2.jpg "Fit Lane"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

All the code for this project is contained in the IPython notebook located in "./advanced_lane_lines.ipynb".


### Camera Calibration


I simply iterate over all images under camera_cal to extract object points and image points. 

![Original Image][image6]
![Image with Corners][image7]

I managed to extract corners for 17/20 imgaes. There are 3 other images do not have (9, 6) corners, but this does not impact the computation of camera matrix & distortion coefficients. 

I then applied them to all chessboard images.

![Undistorted Image][image8]


### Thresholding


Firstly undistoring the image.

![Original Image][image1]
![Undistored Image][image2]

I used all techniques learned from the course: absolute sobel threshold for x and y, magnitude threshold, direction threshold, hls threshold, etc.

I tried out combinations of different thresholding techniques, each I tuned with different sets of parameters. However, there are some incorrect pixels other than lane still included. Then I applied the technique (region of interest) learned from the first project, then I was able to produce binary image with lanes.

![Binary Image][image3]


### Perspective Transform


I extracted the verstices from an example image and applied it to perform perspective transform. After tuning source/destination points, I was able to transform it as below.

![Warped Image][image4]
![Perspective Image][image9]

The transform inverse is save to compute inverse image later.


### Fit Lanes

I used sliding window search code provided from the course, which applies a polynomial fit for the lane. Images are included in the 

![Sliding Window Search Image][image10]
![Lane Fit Image][image11]

The transform inverse is saved to compute inverse image later.


### Inverse Image

Again, I used code provided from the course to change warped image back to normal, with lane drawn.

![Output Image][image5]


### Pipeline (single images)

With all functions tested as above, I follow the below step to process images in pipeline:

1. compute calibration matrix and distortion coefficients use check board image
2. undistort image
3. use color & gradient thresholding to generate binary image to detect lanes.
4. warp the image to get the bird view image.
5. apply polynomial fit for the lane 
6. inverse image back to normal with lane drawn
7. compute radius and center offset

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I spent many time on tuning parameters (thresholding, vertice points, etc) and I think the hard part for this project is when there is some dark area or unclear stuff on the road, it would impact the calculation of an image frame. 

Due to the time limit, as the my term has already ended and I only have less than 1 week for the extenstion, I don't have enough time for improvement. One possible improvement I could think about is to use successive previous frames to compute points to fit lanes. Hope I could take sometime to do this after the term ends.

Any advice is appreciated. Thank you!