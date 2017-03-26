**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Solid WhiteRight
------------------------------------------

For the solid white right video, Firstly, I used
clip = VideoFileClip("test_videos/solidWhiteRight.mp4")
new_clip = clip.fl_image( white_line )

to split up my video into images before passing the images to my processor functions

My original processor for detecting the line segments consisted of the following steps:

1)Create a polygon to map the interesting area of the image ( our lane )

2)Create RGB thresholds for the color white

3)Find the common section betwen 1) and 2) and color it red, these are our lanes

4)Add the output of 3 back on to our original image

This gave a very acceptable line segment based lane detect of the broken white line lane. The issue with this approach was that the requirement of the solid line detect wasnt met. I could not think of another approach exceot redoing my processor function, this time using CV2 functions.

With the CV2 approach, this is what my pipeline line looks like:

1) covert the image into grayscale
2) Perform Gaussing smoothing
3) Detect edges using canny
4) Create a polygon covering the interesting area (our lane)
5) Map the polygon onto the canny output image to filter out the rest of the image outside our lane for processing
6) Use houghlines to detect our lane lines. I used multiple tries to find suitable params to pass to the houghline function
7) project tthe houghline out put on the original image addWeighted function
8) Return the processed image


Solid Yellow Left
-------------------------------------------------

For the solid yellow left, I again used

VideoFileClip
clip.fl_image( yellow_line )

to split up my video into images before passing the images to my processor functions


my pipeline line looks similar to the white line, except for the "polygon" creation for lane area detection. Because the video shot on a curving road, I used different polygon vertices depending on what point of the video the image belongs to. I keep changing the polygon size as the vehicle curves around the road.

1) covert the image into grayscale
2) Perform Gaussing smoothing
3) Detect edges using canny
4) Create a polygon covering the interesting area (our lane). The vertices of the polygon change depensing upon what point of the video the image belongs to
5) Map the polygon onto the canny output image to filter out the rest of the image outside our lane for processing
6) Use houghlines to detect our lane lines. I used multiple tries to find suitable params to pass to the houghline function
7) project tthe houghline out put on the original image addWeighted function
8) Return the processed image



###Potential shortcomings with my pipeline

For the solid white line video,  I would have liked to stick to my original processor logic of detecting the line segments and somehow connect the line segments together

For the solid yellow line, I need an elegant way to detect curves on the road to modify my polygon for lane area detection

