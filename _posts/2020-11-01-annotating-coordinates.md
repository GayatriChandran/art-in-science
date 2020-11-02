---
layout: post
title: How to annotate a picture with Python
---

In this post, we will learn how to annotate objects in an image, given their coordinates.

### Import libraries

```python     
import pandas as pd
import numpy
import matplotlib.pyplot as plt
```  
###  Load image
```python  
if (__name__ == "__main__"):

    # Load the image
    file = 'test.png'
    im = plt.imread(file)
```
[Spots on black background](assets/img/test_image.png "An example image")

### Load coordinates of objects in image  
```python
    # Load the set of coordinates [object locations] 
    df = pd.read_csv('test.csv')
```


    # Overlay a scatter plot over the image, with numbering
    fig, ax = plt.subplots()
    implot = plt.imshow(im, cmap='gray')
    plt.scatter(df['x'],df['y'], marker="s", s=150, facecolors='none',
                edgecolors='white')
    for i, txt in enumerate(df['index']):
        ax.annotate(txt+1, (df['x'][i], df['y'][i]), color='white', 
                    size=8, xytext=(2, 7), textcoords='offset points')
    plt.axis('off')

    # Save the figure
    plt.savefig(file_name+'_marked.png', bbox_inches = 'tight', pad_inches = 0)
```
