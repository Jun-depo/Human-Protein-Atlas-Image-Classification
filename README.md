# Human-Protein-Atlas-Image-Classification
## Human Protein Atlas Image Classification (Kaggle competiton).

### Introduction

  Different proteins are localized at different locations inside the cell. Protein localization patterns are detected and revealed by antibody stainings shown in the green channel of images, along with microtubules (red), endoplasmatic reticulum (yellow) and nuclei (blue) stainings.  Based on labels of the training set, classification models need to learn to distinguish 28 different protein localization patterns.  Each protein can be localized at multiple subcellular locations, carrying multiple labels.  
 (Link:  https://www.kaggle.com/c/human-protein-atlas-image-classification)
 
  Iniatial data analysis showed that there are many examples of proteins localized at nucleoplasm, cytosol, plasma membrane, nucleoli, mitochondria, golgi apparatus and nuclear bodies.  Very few examples of proteins localized at aggresome, mitotic spindle, lipid droplets, peroxisomes, endosomes, lysosomes, microtubule ends, rods & rings as shown in the figure below. 
  
![alt text](https://github.com/Jun-depo/Human-Protein-Atlas-Image-Classification/blob/master/count1.png)

### Over sampling underrepresented class examples
I oversampled underrepresented classes in training dataset through multiplication of existing samples (2-7 times of original samples depending on the classes) that partially offset unbalanced sample distribution.  

### Data Generator.

Total amount of image data is larger than the system memory, data generator was used to retrieve and feed image data in batches, I used the tutorial (https://stanford.edu/~shervine/blog/keras-how-to-generate-data-on-the-fly) as the main reference for the data generator.   

### Image augmentation and cropping.

In the data generator, I added a random choice of image augmentations from 4 different rotations (0, 90°, 180°, 270°), vertical or horizontal flipping. I only used one GTX 1080 ti, training large numbers of images at 512X512 was very slow and unstable. I random selected an image crop of 255X255 pixels for each image sample instead of downsizing images. 

### External data

Beside official Kaggle train data, additional data were available from https://www.proteinatlas.org/. I followed some ideas from the [discussion forum](https://www.kaggle.com/c/human-protein-atlas-image-classification/discussion/69984#430860) to obtain and process external data. High resolution images were down-sampled to 512X512 pixels, to match the image size of official training samples. 

### The image classification method. 

Since each image can have more than one label.  Softmax function couldn't be used for the highest probabilty class.  Binary classification on each of 28 labels were used to predict whether the image contains each of the labels.

Single model method were used for the classification.  I tried InceptionResNetV2 and ResNet50. InceptionResNetV2 worked better under my  primary testing, I preceeded with InceptionResNetV2. I initially used pretrained InceptionResNetV2 model which I could only input 3 image channels. I switched to non pretrained model, that allowed me to feed all 4 channnels of image data. 

### Protein localization label prediction in the test data.

I classified labels for each images with 8 random crops of that image, then took max probabilty for each label out of 8 predictions (from 8 crops). Initially, I calculated mean probabilty for 8 random crops.  Then, I thought certain crops might contain more information for label classification. I didn't have time to test which method (mean or max) was better way for the classification.  

I got external data one day before the completion of the competition.  I could only train 12 epochs with extra external data (I loaded the previously self-trained weights without extra external data). I run about 12 more epochs post the competition, and got better results than my official submission.   

### Script links.
#### Data Visualization:  
https://github.com/Jun-depo/Human-Protein-Atlas-Image-Classification/blob/master/Protein_Image_Classification_Visualization.ipynb
#### Image Classification:
https://github.com/Jun-depo/Human-Protein-Atlas-Image-Classification/blob/master/protein_subcellular_localization_classification.ipynb
