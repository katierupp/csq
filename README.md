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

## References 
This project was inspired by the Thermimage package, which allows for FLIR thermal image analysis in R: 

Glenn J. Tattersall. (2017, December 3). Thermimage: Thermal Image
Analysis.doi: 10.5281/zenodo.1069704 (URL:
<http://doi.org/10.5281/zenodo.1069704>), R package, &lt;URL:
<https://CRAN.R-project.org/package=Thermimage>&gt;.
[![DOI](https://zenodo.org/badge/33262273.svg)](https://zenodo.org/badge/latestdoi/33262273)
