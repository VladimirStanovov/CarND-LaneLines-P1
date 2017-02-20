#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Pipeline description

The pipeline contains the following steps:
	-blur the image with gaussian blur (kernel size 3)
	-apply Canny edges detection to a blurred image
	-cut the region of interest using trapezoid vertex
	-apply Hough algorithm to detect lines
	-put the lines from previos step on the original image (this could be removed)
	-run the lanes detection algorithm
	-draw lanes on original image using weighted_img function
The lanes detection algorithm is implemented as follows:
	-find k ("gradient") and b ("position") coefficients of every line (x=k*y+b equation)
	-apply gradient filtering - get rid of horisontal or vertical lines if any
	-separate gradients in two groups while filtering - left and right lane
	-remember the "upper" position of every lane by calculating min over all lines for every lane
	-average over gradients and positions for every lane (median values used instead of average)
	-apply exponential smoothing (optional, prevents lines oscillations)
	-finally, draw the lines using the parameters we got

Grayscale conversion was eliminated as it delivered negative effect.


###2. Potential shortcomings

In case if the image is turned or skewed by some degree, the algorithm will probably fail due to the fact that the vertex is fixed, and it may cut off the right region. 
Moreover, the lane detection algorithm relies on filtering Hough lines based on gradient values, turning the image will break this part, resulting in loss of one or even both lanes. 
The whole pipeline is not good enough at recognizing turns on a road - which could be observed on the additional challenge example.
Different lighting conditions may also cause troubles (although I've tested the algorithm on my own video, and it worked).

###3. Possible improvements

A possible improvement would be to use more complex functions to approximate lanes, i.e. not just k*x+b, but probably quadratic or other equations.
One more suggestion is to run the whole pipeline several times with different parameters and vertexes, and average results, this may improve robustness to image turns, shaking, etc.
