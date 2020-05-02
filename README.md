# **Advanced Lane Finding Project** 

## Writeup


The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

### Pipeline (single images)

#### 1. Camera Calibration and Distortion Correction

I used "cv2.findChessboardCorners" function to calibrate the camera. Using "cv2.undistort", undistorted the calibration image. 

[Image1]: ./output_images/Undistorted_Image_Chessboard.png "Chessboard Undistorted Image"

#### 2. Gradient and Color Transform

I used a combination of color and gradient thresholds to generate a binary image. Here's an example of my output for this step.
[Image2]: ./output_images/Gradient_x_threshold.png "X threshold"
[Image3]: ./output_images/Gradient_y_threshold.png "Y threshold"
[Image4]: ./output_images/Magnitude_threshold.png "Magnitude threshold"
[Image5]: ./output_images/Color_threshold.png "Color threshold"
[Image6]: ./output_images/Combined_threshold.png "Combined threshold image"

#### 3. Perspective Transform.

The code for my perspective transform includes a function called `warp()`, which takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

src_coordinates = np.float32(
            [[280,  700],  # Bottom left
             [595,  460],  # Top left
             [725,  460],  # Top right
             [1125, 700]]) # Bottom right
        
dst_coordinates = np.float32(
            [[250,  720],  # Bottom left
             [250,    0],  # Top left
             [1065,   0],  # Top right
             [1065, 720]]) # Bottom right 

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

[Image7]: ./output_images/Warped_image.png "Warped image"

##### 3.1. Detect Lane Pixels and Find Lane Boundary

Then I did sliding window search and fit my lane lines with a 2nd order polynomial using `detect_lines()` 

[Image8]: ./output_images/Lane_lines_detected.png "Sliding window search"

#### 4. Warp Detected Lane onto Original Image

I plotted detected lanes on blank image using 'cv2.fillPoly'.

[Image9]: ./output_images/Lane_detected_with_metrics.png "Detected lanes on original image with metrics"

---

### Pipeline (video)

Here's a
I tried to tune my parameters for both optional challenge videos. It worked fairly well on challenge video and average on harder challenge video.

 [Video1](./project_video_output.mp4 "Project Output Video") 
 [Video1](./challenge_video_output.mp4 "Harder Challenge Output Video")
 [Video1](./harder_challenge_output_video.mp4 "Challenge Output Video")

---

### Discussion

This has been a very challenging project and I am happy with the results.

There is still some room for improvements:
Perform some sanity checks - 
1. Check if lanes have similar curvature
2. Check that lanes are seperated by approximately right distance horizontally.

Pipelines might fail if car is too close to host vehicle and either of the lanes.