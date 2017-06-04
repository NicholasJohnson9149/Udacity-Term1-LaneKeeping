#**Finding Lane Lines on the Road** 

##Writeup Template
##By Nicholas Johnson 
##May 18th 2017 
## Udacity Self Driving Car Nano-Degree P1 

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


**Finding Lane Lines on the Road**

Using Open CV and some clever masking it’s possible to filter lane lines from video feeds collected from almost any dashboard camera. The gaol of this assignments to familiarize one with the development environments of the UDacity class, Anaconda and Jupyter Notebooks, while also developing a pipeline to detect and mask lane lines for self driving cars to follow. 

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The lessons are clear and concise with great detail where needed and lots of extra links to learn more if you want to dive into the rabbit hole. Using pre recored footage and some great examples you start to peace together the pipeline to create your own self driving lane keeping image software.  The ideas simple enough take raw video of a car driving down different sections of Highway 280 and apply filters and mask to the image to extract the most important information, the lane lines. 
The pipeline started with a raw video, but considering that a video is nothing more than a collection on images we start by breaking down the image into its components. See a picture is made of of pixel, and each pixel has a red, green, and blue value attached to it.  In a computer colors are represented by bits so when a image has 8 bit color accuracy it means that each pixel, RGB has 256 different possible states that it can represent. Think of this image as a set of 3 layers red, green ,blue all overlaid onto of each other to achieve the final result.  These facts of digital images leads to ethics’s first part of the pipeline. 

Converting the RGB to grayscale is set one in setting up the image for the next steps in tracking lanes lines. Using Open CV this tack is simple, because others have done this so many times we call a function that take step images passed into it and converts them to a gray scale image that we then apply the next part of the pipeline to. 

Applying the Gaussian blur to smooth out the image because we want to look for the edges of similar objects. The reason we want to blur the image at all is because white lane line might have some gradient in colors next to each other so when we apply the next process to the image we could end up overfitting edges and most finding the entire lane line. This effect smooths the color differences between adjacent pixel so that we can apply the next process. 

The third step requires use to use a function in open CV called Canny edge detection, this looks at the gradient of pixel next to each other and if the difference is some magnitude grader or less it figures that gradient as an edge. Using Canny edge detection takes the image form a slightly blurred grayscale to a black and white image with white outlines of what should be the edges of everything in the picture, including the lane lines. 

Because we don’t want to consider other objects like, trees, other cars, or budding, we now need to focus where we look on the image for lane lines. Step four is masking the image around lane lines. Note this makes an assumption that the camera taking video is always in the same location on the car, and that the mask relatively covers enough space to find both narrow and wide lane lines without accidentally identifying other anomalies. 
 
Once a satiable array has been found on the image to create a polygon that wraps around just the lane lines you can then use an Open CV function to find the lane lines in that section and overlay a color onto them. It seemed like this class was using red in al the examples which makes logical sense because its a color not found in nature all that often. 

The last part of image processing is to used a mathematical principle converting image pixels into Hough space. This takes a line plotted in (x,y) space and converts it to a point in Hough space (m,b). A single point in image space has many possible lines that pass through it, but not just any lines, only those with particular combinations of the m and b parameters. Rearranging the equation of a line, we find that a single point (x,y) corresponds to the line b = y - xm. This is important to finding lane lines because a box, like a lane line will have a point in Hough space allowing the mask to clearly identify lines in the masked region. 


After all this we are left with a bunch of images that are no longer recognizable, one is just the black and white edges of the image another that has masked and colored the detected image with where it believes the lane is using Hough space to distinguish the areas of interest.  Finally we want to overly the results into the original image and see how well our pipeline tracks lane lines in a wide range of settings, like night driving, curvy roads, and city streets. 

###2. Identify potential shortcomings with your current pipeline

This simple masking is only a small peace of detecting lane lines in autonomous cars because it will breakdown in a wide range of corner cases, if the road become stop curvy or lane lines are painted poorly on the road. Even bike lanes will confuse this over simplified lane line detection strategy. So to improve this we would create a number of pipelines for different applications and apply machine learning to figure out what pipeline would best detect lane lines in a verity of applications. Another key to autonomous cars for helming keep better lane lines is to use GPS at a local level to create redundancy in its image processing and better find where it thinks lane lines should be. 

### Reflection

In conclusion the lane line pipeline we created in this assignment process and image in this order to find and mask lane lines. 
1. Convert RGB to grayscale
2. Apply a slight Gaussian blur
3. Perform Canny edge detection
4. Define a region of interest and mask away the undesired portions of the image
5. Retrieve Hough lines 
6. Apply lines to the original image

Once this has been competed we just apply this process to every image in the videos to create a video that tracks lane lines in specific cases, like a sunny freeway. 

###3. Suggest possible improvements to your pipeline

So what about all those coiner cases, well that becomes more difficult as this strategy would need to be modified for them and then each pipeline would be switched between to best suit the needs of the system. Another interesting thing is that this is not an optimal suction, when I tried to test this on footage i took driving in SF on a go pro stuck on the hood of my truck, it struggled to deal with lane changes, and bike lanes because it often confused them as useable lanes. Because the mask needs to be narrow enough not to detect lane lines a lane over or too far ahead the pipeline in effect overfits to very specific road conditions. 

To improve this, we would want to play, or optimize the perimeters that effect each section of the pipeline using a larger data set and figure out what works best across a wider range of lighting, and road conditions. 


