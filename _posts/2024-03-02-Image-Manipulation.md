---
layout: post
title: Image Manipulation
image: "/posts/camaro.jpg"
tags: [Python, Colors]
---

This is a demonstration of some of the ways you can manipulate an image just with NumPy

---

Here's the original image that we'll be playing around with:
<br />
<img src="/img/posts/camaro.jpg" width="150" style="float: left; margin-right: 1000px;" /> 
<br />
    
First we start by importing various the libraries we'll need for this task. We used matplotlib to visualize the images as we were coding, but of course it wasn't needed for the manipulations themselves.

```python
import numpy as np
from skimage import io
import matplotlib.pyplot as plt
```
Then let's read the image and save it as a variable **camaro**

```python
camaro = io.imread("camaro.jpg")
```
This gives us the image as a NumPy array, the shape of which we can see like so:

```python
camaro.shape
>>> (1200, 1600, 3)
```
We can see that there are 1200 rows and 1600 columns of pixels, as well as 3 dimensions, corresponding to the RGB colors.

The first thing we can do is crop the image to only get the part with the car. We do this by slicing the array on the rows and the columns.
We maintain a ":" in the last index to keep all the colors

```python
cropped = camaro[350:1100, 200:1400, :]
plt.imshow(cropped)
plt.show
```
This gives us:

<img src="/img/posts/camaro_cropped.jpg" width="150" style="float: left; margin-right: 1000px;" />

<br />

Next we can try flipping the image vertically. This is done again by using the indexes of the array:

```python
vertical_flip = camaro[::-1, :, :]
plt.imshow(vertical_flip)
plt.show
```
This yields:
<img src="/img/posts/camaro_vertical_flip.jpg" width="150" style="float: left; margin-right: 1000px;" />

A further step we can take is to manipulate the colors so that the image is shown with only its Red, Green, or Blue shades.
We do this by first creating an array - **red** containing only zeros, but taking exactly the same shape as the original image.
Then we take all of the rows and columns from **camaro** but *only* from dimension 1 (or index 0 which corresponds to the Red shade), and set that equal to **red**.
This means that **red** now contains all of the row and column information from **camaro**, but *only* as it pertains to the Red shade.

```python
red = np.zeros(camaro.shape, dtype = "uint8")
red[:, :, 0] = camaro[:, :, 0]
plt.imshow(red)
plt.show
```
<img src="/img/posts/camaro_red.jpg" width="150" style="float: left; margin-right: 1000px;" />

We can then do the exact same process for the Green and Blue shades:

```python
green = np.zeros(camaro.shape, dtype = "uint8")
green[:, :, 1] = camaro[:, :, 1]
plt.imshow(green)
plt.show

blue = np.zeros(camaro.shape, dtype = "uint8")
blue[:, :, 2] = camaro[:, :, 2]
plt.imshow(blue)
plt.show
```

Finally, we can use **vstack** to combine all of these three images together into one large rainbow image!

```python
camaro_rainbow = np.vstack((red, green, blue))
plt.imshow(camaro_rainbow)
plt.show
```
Yielding: 
<img src="/img/posts/camaro_rainbow.jpg" height="100" style="float: left; margin-bottom: 1000px;" />


