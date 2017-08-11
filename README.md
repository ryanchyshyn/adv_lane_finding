## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


This project goal is to write a software pipeline to identify the lane boundaries in a video. This document will describe the algorithm in details.

The Project
---

The key steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./camera_cal/calibration2.jpg "Camera calibration"
[image2]: ./output_images/image1.png "image"
[image3]: ./output_images/image2.png "image"
[image4]: ./output_images/image3.png "image"
[image5]: ./output_images/image4.png "image"
[image6]: ./output_images/image5.png "image"
[image7]: ./output_images/image6.png "image"
[image8]: ./output_images/image7.png "image"
[image9]: ./output_images/image8.png "image"
[image10]: ./output_images/image9.png "image"
[image11]: ./output_images/image10.png "image"
[video1]: ./project_video.mp4 "Video"

### Camera Calibration

I performed camera calibration using the single image ![image][image1] and got camera calibration matrix:
```
Camera calibration matrix: [[ 872.5223319     0.          649.38554509]
 [   0.          853.10470045  213.74930061]
 [   0.            0.            1.        ]]
 ```
 Original images: ![image][image2]
 Undistorted images: ![image][image3]
 
 The difference is not so big but it can be noticable further if not performed right now.
 
 ### Perspective
 
 To get perspective matrix I performed the following calculations:
 ```
     srcPoints = np.float32(
        [[w * 0.18, h * 0.95],
         [w * 0.457, h * 0.63],
         [w * 0.543, h * 0.63],
         [w * 0.82, h * 0.95]])
    dstPoints = np.float32(
        [[w * 0.25, h],
         [w * 0.25, 0],
         [w * 0.75, 0],
         [w * 0.75, h]])
    
    M = cv2.getPerspectiveTransform(srcPoints, dstPoints)
    Minv = cv2.getPerspectiveTransform(dstPoints, srcPoints)
```

and checked the result using *straight* image: ![image][image4]
vertical lines on the first image shows that perspective matrix is correct.

### Pipeline
Next steps are performed for every frame in the video.

## Warping
Warping is performed using M matrix. The result is displayed on previous image.

## Binarization
After many experiments I found the following binarization algorithm is the best:
1. Convert image to HLS color space
2. Get L and S channels: ![image][image5]
3. Adjust gamma on L channel: ![image][image6]
4. Sum *gamma(L)* with *S* images using Max function (i.e. get the brightest pixel from two): ![image][image7]
5. Do binary thresholding using values min=110, max=255: ![image][image8]
![image][image9]

## Lines detection
Perform lines detection using provided in the lesson algorithm: ![image][image10]

## Draw lane highlighting
Draw the polygon on warped clear image

## Unwarp the image
Unwarp the image using inverted M matrix

## Combine original and highliting image

## Draw text on the image
The final image: ![image][image11]

### The final video
![video][video1]
