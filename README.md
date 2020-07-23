![png](output_16_1.png)

# Simple Batch Breakpoint generator python

This jupyter notebook script will generate responsive image for lazy-srcset website, maintain aspect ratio, no stretch. Normally I have to generate it via https://www.responsivebreakpoints.com/ or photoscape which a bit slow.

## Story and explaination

I've a created a Vue very-lazy and simple-to-use src set component that similar to img tag with Breakpoint size [200,763,1132,1400] for other project in production (*not in this project) It's work perfectly with this python image generator.

## How to use?

Specify the folder path that have all the image (recursive) and breakpoint width in list below and run this jupyter notebook.

## Import Library


```python
import os, sys
from PIL import Image, ImageFilter
import math 
import glob 
```

## Defined Function


```python
def resize_maintain_aspect_by_width(PIL_Image,new_width):
    '''
    This function get image and resize by width (maintain aspect ratio) without strech
    '''
    width, height = PIL_Image.size
    # if new width is greater than original width, return the image (no strech)
    if new_width > width:
        return PIL_Image
    
    new_height = math.floor(new_width * height / width )
    return PIL_Image.resize((new_width, new_height), Image.ANTIALIAS)

def apply_GaussianBlur(image,radius=20):
    '''
    Apply GaussianBlur with default rad = 10 to image
    '''
    return image.filter(ImageFilter.GaussianBlur(radius=radius))

def savebreakpoint(filename,PIL_Image,breakpoint,formatter="%s_%s"):
    '''
    This function save PIL image by add postfix from specify breakpoint
    '''
    path = filename.rsplit('.', 1)
    path[0] = formatter % (path[0] , breakpoint)
    PIL_Image.save(r'.'.join(path))
    
def generate_image_breakpoints(index,file_path,image,formatter,breakpoint,blur=True):
    '''
    This is function helper for generate breakpoints.
    '''
    # if it's a lowest-resolution image and generate a lazyload blurred image
    if(index==0 and blur== True):
        tmp = resize_maintain_aspect_by_width(image,breakpoint)
        tmp = apply_GaussianBlur(tmp)
        savebreakpoint(file_path,tmp,"blur",formatter)
        
    tmp = resize_maintain_aspect_by_width(image,breakpoint)
    savebreakpoint(file_path,tmp,breakpoint,formatter) 
```

## Config


```python
# set breakpoints width
breakpoints = [200,763,1132,1400]

# set the folder path, Can set to any folder you want.
files = glob.glob('./testbatch/**/*.jpg',recursive = True) 

# format {Filename}_{breakpoint}.jpg eg. car_200.jpg
formatter="%s_%s"

# format example 2 {Filename}_w{breakpoint}.jpg eg. car_w200.jpg
#formatter="%s_w%s"
```

## Run


```python
for file in files:
    im = Image.open(file) 
    for index,breakpoint in enumerate(breakpoints):
        generate_image_breakpoints(index,file,im,formatter,breakpoint,blur=True)
```

### Output: Sample.jpg to Sample_blur.jpg, Sample_200.jpg, Sample_763.jpg, Sample_1132.jpg, Sample_1400.jpg

## Sample Output


```python
from IPython.display import clear_output
import IPython.display as display
from matplotlib import pyplot as plt
%matplotlib inline 

def _plot(_type,fp):
    tmp = Image.open(fp)
    plt.xlabel( "{0} : Size {1:0,.2f} kB".format( _type,len(tmp.fp.read())/1000 ))
    plt.imshow(Image.open(fp))
    plt.grid(False)
    plt.xticks([])
    plt.yticks([])

def show_preview():
    plt.figure(figsize=(15,15))
    plt.subplot(1,2,1)
    _plot("Original",'./testbatch\\IMG_20200101_125629_1.jpg')
    plt.figure(figsize=(20,20))
    plt.subplot(2,3,1)
    _plot("Generated a lazyload blurred image, Width 200",'./testbatch\\IMG_20200101_125629_1_blur.jpg')
    plt.subplot(2,3,2)
    _plot("Width 200",'./testbatch\\IMG_20200101_125629_1_200.jpg')
    plt.subplot(2,3,3)
    _plot("Width 763",'./testbatch\\IMG_20200101_125629_1_763.jpg')
    plt.subplot(2,3,4)
    _plot("Width 1132",'./testbatch\\IMG_20200101_125629_1_1132.jpg')
    plt.subplot(2,3,5)
    _plot("Width 1400",'./testbatch\\IMG_20200101_125629_1_1400.jpg')

```

## Preview


```python
show_preview()
```


![png](output_16_0.png)



![png](output_16_1.png)



```python

```
