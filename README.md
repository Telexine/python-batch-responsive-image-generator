# Simple Batch Breakpoint generator python

This jupyter notebook script will generate responsive image for lazy-srcset website, maintain aspect ratio, no stretch. Normally I have to generate it via https://www.responsivebreakpoints.com/ or photoscape which a bit slow.

## Story and explaination
I've a created a Vue very-lazy and simple-to-use src set component. the property are similar to <img> and I don't need to worry about other property and that component included fixed Breakpoint size [200,763,1132,1400] in other website in production (not in this project) It's work perfectly with this image generator.

# How to use? 
Specify the folder path that have all the image (recursive) and breakpoint width in list and run the script.

# Input 
Sample.jpg  breakspoints = [200,763,1132,1400]
# Output
Sample_200.jpg, Sample_763.jpg, Sample_1132.jpg, Sample_1400.jpg
