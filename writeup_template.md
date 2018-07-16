# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

[image2]: ./test_images_output/solidWhiteRight.jpg "solid white"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps: 
(1) Convert the images to grayscale.
(2) Use Gaussian Blur to reduce noise.
(3) Do canny edge detection.
(4) Appy the mask of interested region.
(5) Use Hough transform to find the line and draw it.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:

Remember the previous end point (it will be different based on the slope), and connect with the current point.
Code is here:

    x_pre_end_pos = None
    y_pre_end_pos = None
    x_pre_end_neg = None
    y_pre_end_neg = None
    
    for line in lines:
        for x1,y1,x2,y2 in line:
            cv2.line(img, (x1, y1), (x2, y2), color, thickness)
            if x_pre_end_pos is not None and y_pre_end_pos is not None and (y1-y2)/(x2-x1)>0:
                cv2.line(img, (x_pre_end_pos, y_pre_end_pos), (x2, y2), color, thickness)
            if (y1-y2)/(x2-x1)>0:
                x_pre_end_pos = x1
                y_pre_end_pos = y1
            if x_pre_end_neg is not None and y_pre_end_neg is not None and (y1-y2)/(x2-x1)<0:
                cv2.line(img, (x_pre_end_neg, y_pre_end_neg), (x2, y2), color, thickness)
            if (y1-y2)/(x2-x1)<0:
                x_pre_end_neg = x1
                y_pre_end_neg = y1

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image2]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when in the vedio, the region of interested will keep changing.
If the region of interested is fixed, sometime it will include the edge of the front cars.

Another shortcoming could be, when in the curve case, the slope of the line is not fix, draw single line based on slope will not work.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to select region of interested based on each group of frames.

Another potential improvement could be to draw single line not only based on slope, but also other feature.
