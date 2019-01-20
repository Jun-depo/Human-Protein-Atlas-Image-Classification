# Human-Protein-Atlas-Image-Classification
## Human Protein Atlas Image Classification (Kaggle competiton).

### Introduction

  Different proteins are localized at different places inside the cell. Target protein localization patterns are labeled by antibodies that is revealed in the green channel of images, along with microtubules (red), endoplasmatic reticulum (yellow) and nuclei (blue).  Based on lables of the training set, classification models need to learn to distinguish 28 different protein localization patterns.  Each protein can be localized at multiple cellular locations, carring multiple labels.  
 (Link:  https://www.kaggle.com/c/human-protein-atlas-image-classification)
 
  Iniatial data analysis showed that there are many examples of proteins localized at nucleoplasm, cytosol, plasma membrane, nucleoli, mitochondria, golgi apparatus and nuclear bodies.  Very few examples of proteins localized at aggresome,         mitotic spindle, lipid droplets, peroxisomes, endosomes, lysosomes, microtubule ends, rods & rings as shown in the figure below. 
  
![alt text](https://github.com/Jun-depo/Human-Protein-Atlas-Image-Classification/blob/master/count1.png)

### Over sampling underrepresented class examples
I oversampled underrepresented class training examples (2-7 times of original size based the classes) to offset a bit of unbalanced sample distribution. 

### Data Generator.

Total amount of image data is more than system could handle, I used data generator to retrieve and feed image data in batches, I used the tutorial (https://stanford.edu/~shervine/blog/keras-how-to-generate-data-on-the-fly) as the main reference for the data generator.   

### Image augmentation and cropping.

In the data generator, I added a random choice of image augmentations from 4 different rotations (0, 90°, 180°, 270°), vertical or horizontal flipping. I only had one GTX 1080 ti, training large number of images at 512X512 was very slow and unstable. I used image cropping instead reducing image size. 

### External data

Beside official Kaggle training, additional data were available from https://www.proteinatlas.org/. I used some ideas from the discussion forum to get and process external data,  https://www.kaggle.com/c/human-protein-atlas-image-classification/discussion/69984#430860.  High resolution images were reduced to 512X512 pixels, identical to the size of official training images. 

### The image classification model. 

Since each images can have more than one label.  I can't use softmax function for highest probabilty class.  I used binary classification for each of 28 labels to predict whether the image contains that label.

I used single model method for the classification.  I tried InceptionResNetV2 and ResNet50. InceptionResNetV2 worked better under the my limited testing condition, I preceeded with InceptionResNetV2. I initially used pretrained InceptionResNetV2 model which I could only fed 3 image channels. I switched to non pretrained model, that allowed me to feed all 4 channnels of image data. 

### Protein localization label inference in the test data.

For each image, I classified labels for 8 random crops, took max probabilty for each label out of 8 crops. Initially, I calculated mean probabilty for 8 random crops.  Then, I thought certain crops might contain more information for label classification. I didn't have time to test which method (mean or max) was better for the classification.  

I got external data one day before the completion of the competition.  I could only train 12 epochs with extra external data (I loaded the previously self-trained weights with official train data). I run about 12 more epochs post the competition, did get better results.   

