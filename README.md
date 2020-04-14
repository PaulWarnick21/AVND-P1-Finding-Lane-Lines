# Finding Lane Lines
#### Autonomous Vehicle Nano Degree - Project 1

[image1]: ./test_images_output/output_solidWhiteRight.jpg "Description"

## Description

The purpose of this project was to develop a program that takes in a video stream of a vehicle driving on a road and by using computer vision, determine where the edges of the current lane are:

![Description Image][image1]

## Implementation

In order to complete this project I've implemented a pipeline with 7 distinct stages that result in blue lines being drawn on the current lane edges:

1. Read the current frame of the video as an image and determine it's size in pixels
2. Convert the image to gray scale, this helps in step 4 by making edge detection easier
3. Apply guassian noise reduction in order smooth out any pixalation and reduce the amount of errorenous edges we detect in step 4
4. Apply the Canny Edge Detection algorithm to identify the all strong edges in the current image
5. Select a region of interest for the image by identifying approximately where the lanes will start and end. This results in a trapezoid like polygon showing only the current lane while the rest of the image is turned to black.
6. Apply a Hough Transformation in order to detect the actual lines that form the lane edges.
   * During this transformation, instead of drawing each detected line onto the existing image. I calculated an average line that represents the left and right lane edge. This was done by determining the slope of each line as well as the equation for the line itself. From here I was able to pick an approximate y location of where the average line should start and end (540px and 325px) and use this to determine the location of x for each line at these points. I grouped lines by their slope (because from the prespective of the camera each lane edge will have a distinctive slope) and then averaged out the (near and far) x positions for both left and right edges.
7. Finally, after preforming the Hough Transform, I simply drew the averaged lines onto the original image. This occurs for each frame of the input stream resulting in the videos under ```./test_videos_output/solidWhiteRight.mp4```

## Potential Shortcomings

There are a number of issues that could arise by relying entirely on the slopes of the lines detected infront of the vehicle. For example:
 - If a large section of road in under construction and has no lane markings
 - If the road conditions are not ideal (snow, rain, glare, etc.) the program could produce incorrect results

## Future Improvements

Some future improvements for this project could be to adjust the algorithm parameters for Canny, Hough, and the draw lines function in order to more accurately detect what is a line and what is not. Currently the program is unable to determine sharp curves and sometimes wrongly indentifies objects on the road as lines (which has a noticable affect on the average lane position).

