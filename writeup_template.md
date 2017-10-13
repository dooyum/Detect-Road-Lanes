# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflecting on my work with a written report


[//]: # (Image References)

[image1]: ./examples/sample1.jpg "Grayscale with Gaussian blur"
[image2]: ./examples/sample2.jpg "Canny applied"
[image3]: ./examples/sample3.jpg "Region of interest specified"
[image4]: ./examples/sample4.jpg "Hough lines on canny image"
[image5]: ./examples/sample5.jpg "Averaged Hough lines on canny image"
[image6]: ./test_images_output/solidWhiteCurve.jpg "Detected lanes"
[video1]: ./test_videos_output/solidWhiteRight.mp4

---

### Reflection

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied a Gaussian blur with a 3x3 kernel size. The Gaussian blur helped smothen over any jagged edges left over in the grayscale image.

![alt text][image1]


After this I applied a Canny transform on the grayscale image in order to derive a sharp contrast between lines and the surrounding

![alt text][image2]

I applied a region of interest to the canny image in order to ignore lines that were not important.
Special consideration was made to ensure this area of interest was a dynamic factor of the image size. This way the pipeline should be able to apply to images of a different dimension.
![alt text][image3]

The next step was to detect all the lines that make up the road using a Hough transform. With the parameters I provided, the hough transform successfully detected most of the edges that made up the lane lines.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by splitting it into two functions. The first function (houhg_lines()) was responsible for detecting the Hough lines and the second function(draw_lines) was soley responsible for drawing the detected lines on the canny image. I also increased the thickness to 20 pixels, so that it could stand out clearly.
![alt text][image4]

After getting the Hough lines, I applied an averaging function to extrapolate the detected lines into two solid lines, one for each border of the lane. This extrapolation worked by:
	1. Spliting all the detected lines into negative and positive slopes. These correspond to the Left and Right lanes.
    2. Eliminating any Hough lines that did not fall within a reasonable slope threshold.
    3. Finding a line function that could be applied to most of the remaining lines.
    4. Using the image height as y1 and the top of our region of interest as y2, I extrapolated and got two solid lines for the left and right borders of the lane.
![alt text][image5]

With both lane border clearly defined, I overlayed the lanes on the original image.
![alt text][image6]

I tested the completed pipeline by running it on a short video, and it was able to detect the lanes being driven on.


### 2. Potential shortcomings with the current pipeline


One potential shortcoming would be what would happen when there are conrete sections of the road which are lighter in color than a tarred road.

Another shortcoming could be a driving on a road with steeper curves


### 3. Possible improvements to the pipeline


A possible improvement would be to dynamically modify the canny transform thresholds to match the average color tone of the region of interest.

Another potential improvement could be to apply stricter reasonable bounds thresholds to the averaging function.

