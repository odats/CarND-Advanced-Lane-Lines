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

[image1]: ./output_images/undistort_output.png "Undistorted"
[image2]: ./output_images/undistort_output_car.png "Road Transformed"
[image3]: ./output_images/binary_combo_example.png "Binary Example"
[image4]: ./output_images/warped_straight_lines.png "Warp Example"
[image5]: ./output_images/color_fit_lines.png "Fit Visual"
[image6]: ./output_images/example_output.ong "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Camera Calibration


The code for this step is contained in the [1] code cell of the IPython notebook located in "./examples/example.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (The code for this step is contained in the [8] and [10] code cells of the IPython notebook located in "./examples/example.ipynb".  
).  Here's an example of my output for this step.

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp()`, which appears in the [5] code cell of the IPython notebook.  The `warp()` function takes as inputs an image (`img`). Source (`src`) and destination (`dst`) points are hardcodeed in the fuctions body.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32([
[200, 710],
[1100, 710],
[560, 470],
[740, 470]
])

dst = np.float32([
[200, 710],
[1100, 710],
[200, 250],
[1100, 250]
])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

`getLinesFit()` ([45] code cell) - to find initial lines.
`getNextLinesFit()` ([45] code cell) - to find lines when you have initial starting points.

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the [13] and [14] code cells of the IPython notebook located in "./examples/example.ipynb".

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in the [15] code cell of the IPython notebook located in "./examples/example.ipynb" in the function `drawResults()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

Here's a [link to my video result](./test_videos/project_video_result.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further. 

The next distortion can break lines identification:
1) When the road is damaged
2) Specific shadows
3) Traffic jam with a lot of cars

Better filters and more examples can imporve lines identification.

To be honest, lines identification becomes too complicated and can become even more complicated when we will take into account different weather conditions and damages on the road. I believe there should be another completely different approach. I do not believe that dozens hard coded filters with magic numbers will make the algorithm robust enough.
