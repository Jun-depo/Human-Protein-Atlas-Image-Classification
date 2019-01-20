# Human-Protein-Atlas-Image-Classification
## Human Protein Atlas Image Classification (Kaggle competiton).

### Introduction

  Different proteins are localized at different places inside the cell. Target protein localization patterns are labeled by antibodies that is revealed in the green channel of images, along with microtubules (red), endoplasmatic reticulum (yellow) and nuclei (blue).  Based on lables of the training set, classification models need to learn to distinguish 28 different protein localization patterns.  Each protein can be localized at multiple cellular locations, carring multiple labels.  
 (Link:  https://www.kaggle.com/c/human-protein-atlas-image-classification)
 
  Iniatial data analysis showed that there are many examples of proteins localized at nucleoplasm, cytosol, plasma membrane, nucleoli, mitochondria, golgi apparatus and nuclear bodies.  Very few examples of proteins localized at aggresome,         mitotic spindle, lipid droplets, peroxisomes, endosomes, lysosomes, microtubule ends, rods & rings as shown in the figure below. 
  
![alt text](https://github.com/Jun-depo/Human-Protein-Atlas-Image-Classification/blob/master/count1.png)

### Data Generator

Total amount of image data is more than system could handle, I used data generator to retrieve and feed image data in batches, I used the tutorial (https://stanford.edu/~shervine/blog/keras-how-to-generate-data-on-the-fly) as my reference for my data generator.  

### Image augmentation and cropping

In the data generator, I added a random choice of image augmentations from 4 different rotations (0, 90°, 180°, 270°), vertical or horizontal flipping. I only had one GTX 1080 ti, training large number of images at 512X512 was very slow. I used image cropping instead reducing resolution. 

### External data

Beside official Kaggle training, additional data were available from https://www.proteinatlas.org/. I used some ideas from the discussion forum to get and process external data,  https://www.kaggle.com/c/human-protein-atlas-image-classification/discussion/69984#430860.  The images were reduced to 512X512 pixels, identical to official training data.   
