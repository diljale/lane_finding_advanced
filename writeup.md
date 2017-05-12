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

[image1]: ./output_images/undistorted.jpg "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./output_images/binary.jpg "Binary image"
[image4]: ./output_images/warped.jpg "Warp Example"
[image5]: ./output_images/fit_lines.jpg "Fit Visual"
[image6]: ./output_images/final_output.jpg "Output"
[video1]: ./project_video_result.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. I used openCV to compute calibration coefficients.

The code for this step is contained in the first code cell of the IPython notebook located in "./example.ipynb"  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Binary image

I used a combination of color and gradient thresholds to generate a binary image (thrid code example.ipynb).  Here's an example of my output for this step.

![alt text][image3]

#### 3. Tranform lanes.

The code for my perspective transform is in fifth code cell on example.ipynb.  I hardcoded src and destination points for perspective transform from an image where lanes are straight. The approximation that road is flat then makes this assumption valid that i can use the hardcoded points for all other images taken from same perspective.

```python
src = np.float32([[560,475], [724, 475], [1028,670], [270,670]])
dst = np.float32([[offset, offset], [img_size[0]-offset, offset], 
				 [img_size[0]-offset, img_size[1]-offset], 
				 [offset, img_size[1]-offset]])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Identify lane lines

In fourth code cell i extract lane pixels and fit my lane lines with a 2nd order polynomial:

![alt text][image5]

#### 5. Compute curvature and offset from center

I did this in lines 71 through 97 in my code in fourth code cell

#### 6. Result

Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Project video result

Here's a [link to my video result](./project_video_result.mp4)

---

### Discussion

#### 1. Conclusion

1] While my pipeline works good on images and video, it uses few hardcoded paramters which doesn't generalize
2] I used simple techniques for computing binary image and fitting lane lines. One can improve on these techniques by exploring better algorithms
3] I am not using any smoothing or previous frame's result which will improve results for video sequence
4] I would like to improve results on video sequence by adding some filters to image processing pipeline 
