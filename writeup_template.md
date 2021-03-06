## Writeup Template


---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("binary_warped").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./writeup_images/camera_Calibration.png "Undistorted"
[image2]: ./writeup_images/Test_Image_Calibration.png "Test Image Undistortion"
[image3]: ./writeup_images/color_threshold.png "color threshold"
[image4]: ./writeup_images/Gradient_Theshold.png "Gradient Theshold"
[image5]: ./writeup_images/warp_Image.png "Warp Image"
[image6]: ./writeup_images/lane_Marking.png "Lane Marking"
[image7]: ./writeup_images/Curvature_Calculation.PNG "Curvature Calculation"
[image8]: ./writeup_images/Lane_Marking_original_Image.png "Lane Marking original Image"
[image9]: ./writeup_images/Pipeline_Image.png "Pipeline Image"

[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in cell # 2 # of the file called `Advaned-Lane-Line-p4.ipynb`).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result as shown in below image: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like as shown in below image:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at cells # 4 and 6 # in `Advaned-Lane-Line-p4.ipynb`).  Here's an example of my output for this step. 

#### Example of color thresholding:

![alt text][image3]

#### Example of Gradient thresholding:

![alt text][image4]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp_images()`, which appears in cell 9 in the file `Advaned-Lane-Line-p4.ipynb`.  The `warp_images()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[0.15*xLen, yLen], 
    [(0.5 * xLen) - (xLen*0.07), (2/3)*yLen], 
    [(0.5 * xLen) + (xLen*0.07), (2/3)*yLen], 
    [xLen-150, yLen]])
dst = np.float32(
    [[0.15*xLen, yLen], 
    [0.15*xLen, 0], 
    [xLen-150, 0], 
    [xLen-150, yLen]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 192, 720      | 192, 720| 
| 550.4, 480      | 192, 0      |
| 729.5, 480     | 1130, 0      |
| 1130, 720      | 1130, 720|

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image5]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then with the help of sliding_win_search() function and find_lane() function located at cell 14 & 19 respectively, I have find and marked Lane line on image as shown in below image:

![alt text][image6]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

To calculate Lane curvature Radius I used below formula reffered from online tutorial as suggested in class lecture. [Radius of Curvature](http://www.intmath.com/applications-differentiation/8-radius-curvature.php). Code for claculation of curvature radius available in cell 17 in the file `Advaned-Lane-Line-p4.ipynb`.

![alt text][image7]


#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

Finally I implemented Image Processing Pipeline in cell # 25 # in my code in the file `Advaned-Lane-Line-p4.ipynb` in the function `process_image()`.  Here is an example of my result on a test image:

![alt text][image8]

Addition to above pipeline I have added Binary Image, Warp Image and Lane finding Image window stacked with main result image pipeline, It is shown in below image:

![alt text][image9]



---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Please refer link to video output for project video.

[![Project Video](https://img.youtube.com/vi/fJlCvDVxyJo/0.jpg)](https://youtu.be/fJlCvDVxyJo)

Addition to above project video processing I have tested code for challange video and result is shown in below video link:

[![Challange Video](https://img.youtube.com/vi/79LlzLaHOEU/0.jpg)](https://youtu.be/79LlzLaHOEU)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

Although, Project pipeline works well with Project video, there was some issue with challange video like false detection of lane line, it is due to no. of parameter required to tune  if we only depend on computer vision method to detect lane line also the warp prespective points hard coaded, it need to be work out for adaptive approach depend on various road situation, based on the experience on working with above project pipeline, to implement robust Lane finding solution, if we integrate deep learning approach in combination with computer vision, it will give better result as well as avoid false lane detection.
