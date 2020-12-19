# Car Damage Detection using Mask RCNN

This is an implementation of Matterport Mask RCNN trained for car body damage detection.

The goal of this project is to to predict the location and severity of damage to a car given a provided image of the damaged car. This information could be used for faster insurance assessment and claims processing.

### Pre-requisites
- Clone the Matterport repo into this directory [Matterport Mask RCNN](https://github.com/matterport/Mask_RCNN)
- To use pre trained weights for coco, you need to download the wights file and save in this directory [coco weights](https://github.com/matterport/Mask_RCNN/releases/download/v2.0/mask_rcnn_coco.h5)

## Data
Training and validation data was made available through [Kaggle](https://www.kaggle.com/anujms/car-damage-detection#0001.JPEG)

Each train and validation image has been annotated using [VGG Annotator](http://www.robots.ox.ac.uk/~vgg/software/via/via-1.0.6.html) to identify the region of interest (damage)

Images and annotations (json outputs) are stored in the __custom__ directory.

## Training:

Train the base model using pre-trained COCO weights

```
python custom.py train --dataset= custom/ --weights=coco
```

Tensorboard

```
tensorboard --logdir=logs
```

### Sub Directories
- __custom__ directory for images stored in train, test, validation folders as well as the annotations file via_region_data.json

- __logs__ Conatain output of training for car damage instance segmentation. There is one directory for each experiment, with one weights file per epoch per the directory. The weights files are very large (~250MB) and therefore they are not included with this repo.

- __mrcnn__ directory - from the matterport repo. Contains code and utilities for running mask rcnn. I have softlinked to the repo directory `ln -s Mask_RCNN/mrcnn mrcnn`

### Files

- __custom.py__ training file, is set up to override default matterport training configuration and hyper paramaters. Edit this file to fit your needs and configuration.

- __predict.ipynb__ This notebooks takes you through an implementation of training models based on annotated images, as well as code to validate the results.  

  This assumes that images have already been annotated and are in the correct directory (custom/train and custom/val).  

  Weight files for trained models  are stored in the logs directory and can be viewed with tensorboard  

  This technique uses transfer learning - meaning we have to start with a pre-trained model. We use the weights file provided by Matterport which was trained on the CoCo dataset. This file is not contained in the repo because of its large size.

- __example_images.ipynb__ shows images tested on unseen data, alongside original data

- __map_size.ipynb__ shows example of comparing map size of damage against original object detection (car). This could be used to calculate overall cost of damage
