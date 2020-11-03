---
layout: post
title: Simple 'bounding boxes'
# excerpt_separator: <!--more-->
meta-description: This post describes a simple python code to mark and annotate similar objects in an image, given their coordinates (in pixels).  
---

A lot of my work involves analyzing microscopy images that contain tiny biological structures, seen as diffraction-limited spots. Sometimes, it's useful to label these structures as '1', '2', '3', ... etc (or add descriptors like 'in-focus' / 'out-of-focus') to tell them apart post-analysis.  
I wanted to mark these spots and annotate them, and batch process several images in a folder, but couldn't find a straight-forward python implementation (I didn't want to use OpenCV). After some googling and tweaking of code, I settled with this workaround. With `matplotlib`, an image can be overlayed with a scatter plot. And scatter plots are highly customizable.  
Here's my python implementation for marking and annotating similar objects in an image, given their coordinates (in pixels).

<p align="center">
  <img width="300" height="300" src="https://gayatrichandran.github.io/art-in-science/images/test_image.png">
</p>

### Import python libraries

```python     
import pandas as pd
import matplotlib.pyplot as plt
```  
###  Load image
```python  
# Load the image
file = 'test.png'
img = plt.imread(file)
```

### Load coordinates  
The centroid pixel coordinates for these diffraction-limited peaks were obtained from a peak-finding algorithm, and saved in `.csv` format. Here, it's being loaded into a pandas dataframe. 
```python
# Load the set of coordinates [object locations] 
df = pd.read_csv('test.csv')
```

| Index |   x   |   y   |
| :---: | :---: | :---: |
|   0   |  34   |  76   |
|   1   |  84   |  94   |
|   2   |  116  |  124  |
|   3   |  152  |  130  |

## Overlay scatter plot  
I kept searching online for 'bounding boxes' and how to draw rectangles using `pillow`, but this is simpler. With `matplotlib` you can just overlay a custom scatter plot on top of an image.

```python
# Overlay scatter plot on the image, with numbering
fig, ax = plt.subplots()
plt.imshow(img, cmap='gray')
plt.scatter(df['x'],df['y'], marker="s", s=150, facecolors='none',
            edgecolors='white')
for i, txt in enumerate(df['index']):
    ax.annotate(txt+1, (df['x'][i], df['y'][i]), color='white', 
                size=8, xytext=(2, 7), textcoords='offset points')
plt.axis('off')

# Save the figure
plt.savefig('test_marked.png', bbox_inches = 'tight', pad_inches = 0)
```
<p align="center">
  <img width="300" height="300" src="https://gayatrichandran.github.io/art-in-science/images/test_annotated.png">
</p>

Modify `xytext` to change the position of text and `s` to change the size of these markers. I also use `glob` to iterate through multiple similar images in a directory.

### References  
[https://stackoverflow.com/questions/...](https://stackoverflow.com/questions/5073386/how-do-you-directly-overlay-a-scatter-plot-on-top-of-a-jpg-image-in-matplotlib)
