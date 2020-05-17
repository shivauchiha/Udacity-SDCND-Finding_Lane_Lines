# **Finding Lane Lines on the Road** 

## Writeup Template



---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./Results_images/Hough_lines_soft.jpg "Hough_lines_soft"
[image2]: ./Results_images/gaussian_bluring.jpg "Gaussian blurrings"
[image3]: ./Results_images/Range_thresholding.jpg "thresholding"
[image4]: ./Results_images/ROI_crop.jpg "Cropping ROI"
[image5]: ./Results_images/canny_edges.jpg "canny_detection"
[image6]: ./Results_images/output.jpg "Final"
[image7]: ./Results_images/grayscale.jpg "Grey"

---

### Process

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The Pipeline consists of 7 steps for lane detection

**Process I**
First the image is converted into grey scale so it cab processed for application in Range thresholding 

![alt text][image7]

**Process II**

The 3 color channel is converted into single channel , the values of whitness range from 0-255 . A gaussian filter is applied to smoothen the image and remove noise. 

![alt text][image2]

**Process III**
Now this image values are thresholded , anything within a range of 154-255 will be held and rest of the pixel values will be made zero . The thresholding values are arrived using trial and error 

![alt text][image3]

**Process IV**
Despite filtring using thresholding objects like car or road side shrubs still exists in the image , in some cases even skies .
Thus what we do is we select a region of interest within the image anything outside it will be equated to zero. Since we know camera position is fixed thus we can predict which ROI of image the road usually apperas based on that you can define a quadilateral boundry and equating anything outside the boundry as zero. 

![alt text][image4]

**Process V**
We have got a clean image where we can see only our load lanes in the image .Now we perform canny edge detection over it to detect edges . 

![alt text][image5]

**Process VI**
Once edges are detected we try to find straight edges using HoughlinesP . This give us a serious of lines who are potential candidate for straight line . Now we will plot these lines over original image using a weighted function defined in OPENCV 

![alt text][image1]

**Process VII**
Finally we try to seperate the series of lines into 2 groups . 1 group will belong left plane , other to right plane. This done by observing the slopes of each line segment(x1,y1,x2,y2) using slope formula (y2-y1/x2-x1). Any segement with slope greater than zero will belong to right lane and vice versa. Now to draw a stright line in right lane . These are the steps

1.Find midpoints of all linesegement linked to right lane
2.Find the median of slopes of all lines segements associated with right lane
3.we know the maximum and minimum y range on any lane we want to  dete which is determined by ROI dimension we cropped earlier
4. using y=mx+c draw the line

follow the same procedure for left lane as well !
![alt text][image6]

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming is its dependecy on static ROI using cropping . This can actually cause problem in uneven terrain and curves.


### 3. Suggest possible improvements to your pipeline

This piple is very hand engineered and may not be robost to different condition . Since , I have some experience with capabilites of CNNs .I think a NN will do a better job in detecting lane lines even in dynamic conditions .
