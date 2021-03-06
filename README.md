# csq
This repository provides a way for thermal videos acquired by a FLIR camera to be read in Python. Only files with the ```.csq``` extension are compatible with this code.

The ```.csq``` file format stores each image in the thermal video in 16-bit binary form. Using some calibration constants from the thermal camera, the temperature data can be calculated, allowing the image to be expressed in degrees Celcius. With this repository, you can directly obtain the temperature values without having to worry about this conversion! 

## Installation
Coming soon... 

## Tutorial 
Here I will use an example thermal video to show you how to use this repository. If you would like to follow along, you can download ```cat.csq``` from this Google Drive folder: https://drive.google.com/drive/folders/1aT98zkNw8DwJ1ImS4mUrWMhvL5NjCS7S?usp=sharing. 

First, import the module and construct a CSQReader object from the path to ```cat.csq```: 

```python
from csq import CSQReader

path = '/Users/tuthill/Downloads/analysis/videos/cat.csq'
reader = CSQReader(path)
```

To read a frame of the video, you can use the ```next_frame()``` function. Let's read and plot the first frame: 

```python
import seaborn as sns 
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import make_axes_locatable

def plot_thermal(frame): 

    sns.set_style('ticks')
    fig = plt.figure()
    ax = plt.gca()
    im = plt.imshow(frame, cmap = 'hot')
    divider = make_axes_locatable(ax)
    cax = divider.append_axes("right", size="5%", pad=0.05)
    cbar = plt.colorbar(im, cax = cax)
    cbar.ax.set_ylabel('Temperature ($^{\circ}$C)', fontsize = 14)
    sns.despine()
    plt.show()

frame = reader.next_frame()
plot_thermal(frame)
```
![cat](https://github.com/katierupp/csq/blob/main/examples/frame1.png?raw=true)

When analyzing thermal imaging data, you may want to perform a certain operation on each frame. For example, maybe you want to extract the maximum temperature at each frame in the video. To determine this, you can simply create a loop to continuously call the ```next_frame()``` function:

```python
import numpy as np

max_temps = []

while reader.next_frame() is not None: 
    frame = reader.next_frame()
    max_temps.append(np.max(frame))
    # other operations for current frame
```
However, there may be cases when you only need to extract thermal data from only certain frames within a video. For faster processing, you can use the ```skip_frame()``` function to skip over any frames that you do not need any thermal data from. Perhaps we only want to plot the 200th frame:

```python
fnum = 0
while fnum < 200: 
    frame = reader.skip_frame()
    fnum+=1
  
frame = reader.next_frame()
plot_thermal(frame)
```

## References 
This project was inspired by the Thermimage package, which allows for FLIR thermal image analysis in R: 

Glenn J. Tattersall. (2017, December 3). Thermimage: Thermal Image Analysis. doi: 10.5281/zenodo.1069704 (URL:<http://doi.org/10.5281/zenodo.1069704>), R package, &lt;URL:<https://CRAN.R-project.org/package=Thermimage>&gt;.[![DOI](https://zenodo.org/badge/33262273.svg)](https://zenodo.org/badge/latestdoi/33262273)
