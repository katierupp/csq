# csq
This repository provides a way for thermal videos acquired by a FLIR camera to be read in Python. Only files with the .csq extension are compatible with this code.

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

