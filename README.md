# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[original]: ./writeup/output_19_0.png "Original"
[gray]: ./writeup/output_19_1.png "Grayscale"
[blurred]: ./writeup/output_19_2.png "Blurred"
[canny]: ./writeup/output_19_3.png "Canny edges"
[mask]: ./writeup/output_19_4.png "Mask"
[edges]: ./writeup/output_19_5.png "Edges"
[hough]: ./writeup/output_19_7.png "Hough"
[maskedhough]: ./writeup/output_19_8.png "Masked Hough"
[result]: ./writeup/output_19_9.png "Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
My pipeline has 8 steps:

Original image
![original][original]

1. Get the grayscale
![gray][gray]

2. Blur the image
![blurred][blurred]

3. Get the edges from canny
![canny][canny]

4. Use a mask to get an area of interest
![mask][mask]

5. Apply the mask on the result from canny
![edges][edges]

6. Get hough lines
![hough][hough]

7. The hough lines are extrapolated so we need to apply the mask again
![maskedhough][maskedhough]

8. Combine the masked hough lines with the original image
![result][result]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by getting the slope, averaging it and calculating new points for `x1`, `y1`, `x2` and `y2`. If the slope is ± infinite I simply skip those points. If no suitable points are found I don't draw a line.

See the Jupyter notebook for code. See the `test_videos` directory for output videos.

### 2. Identify potential shortcomings with your current pipeline
There are a couple of shortcoming:
- When lines from canny come into the masked area they're wrongly identified as part of the lane lines
- Lines or texture in the road that are not part of the lane lines are detected as lane lines, this could be overcome by adding a second mask on the inside of the lane lines so that only the lane lines themselves are considered at, not the road.
- Shadows over the lane lines make them harder to detect
- Heavily curved road falls outside of the mask, or can't be detected at all, or of it is detected might be wrongly selected as belonging to the opposite lane line


### 3. Suggest possible improvements to your pipeline
Ways to improve:
- Add a second mask on the inside
- Try a totally different approach in combination with this one
- Keep a record of a couple of previously detected lines and use that to update the mask. Look an offset to either side of the previously detected line
- Tune the hyperparameters
