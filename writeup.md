# **Finding Lane Lines on the Road** 

## Writeup

### Reflection

### 1. Pipeline description

The first steps of the pipeline are basically the methods introduced in class:
* convert image to gray scale
* apply gaussian blur
* detect edges using canny edge detection
* limit search space to a region of interes
* use hough transformation to detect lines

Given the lines found we try to identify two groups of lines which represent the left and the right lane using the following steps:

* initialize lane groups with those found in last frame (halve their count)
* for every line, check if it matches to an already added lane group, by:
** computing line parameters m, b
** compare them to the line parameters of the lane group (using empirical threshold, scaling b due to different scale)
* if line matches, update the lane group (count, parameters)
* if line doesn't match to any lane group, add a new lane group containing only this line
* take the two lane groups with highest count as left and right lane (other groups are outliers/ probably no lanes)


### 2. Potential shortcomings with the current pipeline


One shortcoming is the first part of the pipeline: Lanes will not be detected as such if the contrast of lane and street is rather small (due to different lane colors, street color, lightning conditions etc.).

Another shortcoming is the assumption that lanes can be represented by lines in the second part of the pipeline.


### 3. Possible improvements to the pipeline

To handle the first shortcoming, we could try to optimize the different parameters used there. We could also try to use additional information we have about lanes (e.g. typical colors, shape) to differentiate between "random" lines in the picture and the actual lanes.

Considering the second shortcoming, we could adapt our model to allow curved lines (they should probably be convex or concave) which try to match the identified lines (instead of a linear line).
