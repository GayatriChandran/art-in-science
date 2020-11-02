---
layout: post
title: A simple substitute for bounding boxes
---

In this post, we'll see how to annotate simple objects in an image, given their coordinates. Most of the time, I deal with pictures of diffraction limited spots (like the one below), and I'm constantly trying to keep track of which 'spot' is which. Here is a snippet of code that I find useful for quick documentation. 

### Import libraries

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
<!-- ![Spots on black background](assets/img/test_image.png "An example image"){:class="img-responsive"} -->

<p align="center">
  <img width="300" height="300" src="https://gayatrichandran.github.io/art-in-science/images/test_image.png">
</p>

### Load coordinates  
I already have the centroid pixel coordinates saved in csv format. Here, it's being loaded into a pandas dataframe.
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
I kept searching online for 'bounding boxes' and how to draw rectangles using `pillow`, but this is a quick work-around. With `matplotlib` you can easily overlay a custom scatter plot on top of an image.

```python
# Overlay a scatter plot on the image, with numbering
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

Modify `xytext` to change the position of text. You can also use `glob` to iterate through multiple similar images in a directory.

### References  
[https://stackoverflow.com/questions/...](https://stackoverflow.com/questions/5073386/how-do-you-directly-overlay-a-scatter-plot-on-top-of-a-jpg-image-in-matplotlib)
