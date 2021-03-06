# Classification On Chest X-Ray Diagnosis Images

## Task

Given the dataset, the model is implemented based on a classification task. This classification task have two classes; Normal (0), Pneumonia (1). Model is implemented in deep learning library PyTorch and the model is selected as pretrained model VGGNet16. VGG16 is a convolutional neural network model proposed by K. Simonyan and A. Zisserman from the University of Oxford in the paper “Very Deep Convolutional Networks for Large-Scale Image Recognition”. It was one of the famous model submitted to ILSVRC-2014. It makes the improvement over AlexNet by replacing large kernel-sized filters (11 and 5 in the first and second convolutional layer, respectively) with multiple 3×3 kernel-sized filters one after another. Model evaluates that networks of increasing depth using an architecture with
very small (3 x 3) convolution filters, which shows that a significant improvement on the prior-art configurations can be achieved by pushing the depth to 16–19 weight layers. 

<img src="/im/vgg1.png" alt="drawing" width="500"/>

<img src="/im/vgg2.png" alt="drawing" width="500"/>

## Paper

- abs: https://arxiv.org/abs/1409.1556
- pdf: https://arxiv.org/pdf/1409.1556.pdf

## Dataset

The normal chest X-ray depicts clear lungs without any areas of abnormal opacification in the image. Bacterial pneumonia (middle) typically exhibits a focal lobar consolidation, in this case in the right upper lobe , whereas viral pneumonia manifests with a more diffuse ‘‘interstitial’’ pattern in both lungs. The dataset is organized into 3 folders (train, test, val) and contains subfolders for each image category (Pneumonia/Normal). There are 5,863 X-Ray images (JPEG) and 2 categories (Pneumonia/Normal).

Chest X-ray images (anterior-posterior) were selected from retrospective cohorts of pediatric patients of one to five years old from Guangzhou Women and Children’s Medical Center, Guangzhou. All chest X-ray imaging was performed as part of patients’ routine clinical care.

For the analysis of chest x-ray images, all chest radiographs were initially screened for quality control by removing all low quality or unreadable scans. The diagnoses for the images were then graded by two expert physicians before being cleared for training the AI system. In order to account for any grading errors, the evaluation set was also checked by a third expert.

data:

    ├── test
    │   ├── NORMAL
    │   └── PNEUMONIA
    ├── train
    │   ├── NORMAL
    │   └── PNEUMONIA
    └── val
        ├── NORMAL
        └── PNEUMONIA
## Samples From Training Set and Class Frequency

<p float="left">
  <img src="/im/samples.png" width="600" />
  <img src="/im/freq.png" width="400" /> 
</p>

## Model Architecture and Properties

In the model, hyperparameters are chosen; batch size of 8, epoch range of 10, learning rate is chosen based on the VGGNet paper that is linked above. 

The batch size of 8 is a must due to CUDA memory qualification (CUDA is out of memory). You can change it with 128 for better performance.  

        Layer (type)               Output Shape         Param 
            Conv2d-1          [8, 64, 224, 224]           1,792
              ReLU-2          [8, 64, 224, 224]               0
            Conv2d-3          [8, 64, 224, 224]          36,928
              ReLU-4          [8, 64, 224, 224]               0
         MaxPool2d-5          [8, 64, 112, 112]               0
            Conv2d-6         [8, 128, 112, 112]          73,856
              ReLU-7         [8, 128, 112, 112]               0
            Conv2d-8         [8, 128, 112, 112]         147,584
              ReLU-9         [8, 128, 112, 112]               0
        MaxPool2d-10           [8, 128, 56, 56]               0
           Conv2d-11           [8, 256, 56, 56]         295,168
             ReLU-12           [8, 256, 56, 56]               0
           Conv2d-13           [8, 256, 56, 56]         590,080
             ReLU-14           [8, 256, 56, 56]               0
           Conv2d-15           [8, 256, 56, 56]         590,080
             ReLU-16           [8, 256, 56, 56]               0
        MaxPool2d-17           [8, 256, 28, 28]               0
           Conv2d-18           [8, 512, 28, 28]       1,180,160
             ReLU-19           [8, 512, 28, 28]               0
           Conv2d-20           [8, 512, 28, 28]       2,359,808
             ReLU-21           [8, 512, 28, 28]               0
           Conv2d-22           [8, 512, 28, 28]       2,359,808
             ReLU-23           [8, 512, 28, 28]               0
        MaxPool2d-24           [8, 512, 14, 14]               0
           Conv2d-25           [8, 512, 14, 14]       2,359,808
             ReLU-26           [8, 512, 14, 14]               0
           Conv2d-27           [8, 512, 14, 14]       2,359,808
             ReLU-28           [8, 512, 14, 14]               0
           Conv2d-29           [8, 512, 14, 14]       2,359,808
             ReLU-30           [8, 512, 14, 14]               0
        MaxPool2d-31           [8, 512, 7, 7]                 0
    AdaptiveAvgPool2d-32       [8, 512, 7, 7]                 0
           Linear-33           [8, 4096]            102,764,544
             ReLU-34           [8, 4096]                      0
          Dropout-35           [8, 4096]                      0
           Linear-36           [8, 4096]             16,781,312
             ReLU-37           [8, 4096]                      0
          Dropout-38           [8, 4096]                      0
           Linear-39           [8, 256]               1,048,832
             ReLU-40           [8, 256]                       0
          Dropout-41           [8, 256]                       0
           Linear-42           [8, 2]                       514
       LogSoftmax-43           [8, 2]                         0



        Total params: 135,309,890
        Trainable params: 1,049,346
        Non-trainable params: 134,260,544

        Input size (MB): 4.59
        Forward/backward pass size (MB): 1750.23
        Params size (MB): 516.17
        Estimated Total Size (MB): 2270.99
        
## Data Preprocessing

ImageNet statistics are used for normalization and rezizing. For RGB values; mean and standart deviation of training, test and validation set are chosen (0.485, 0.456, 0.406) and (0.229, 0.224, 0.225). Dataset consists of variable-resolution images, while VGGNet requires a constant input dimensionality. Therefore, dataset is down-sampled the images to a fixed resolution of 224 × 224. Given a rectangular image, first is rescaling the image such that the shorter side was of length 224, and the second is cropped out the central 256×256 patch from the resulting image.I did not pre-process the images in any other way, except for subtracting the mean activity over the training set from each pixel. So model is trained our network on the (centered) raw RGB values of the pixels. (Krizhevsky et al., 2012).

## Data Augmentation

I use data augmetation methods in training set to reduce overfitting on image data via artificially enlarge the dataset using label-preserving transformations (Krizhevsky et al., 2012). I use

        torchvision.transforms.ColorJitter(brightness=0, contrast=0, saturation=0, hue=0) 

that randomly changes the brightness, contrast and saturation of an image.

        torchvision.transforms.RandomRotation(degrees=15)
            
that rotates the image by angle. 
            
        torchvision.transforms.RandomHorizontalFlip(p=0.5) 
            
that  orizontally flips the given image randomly with a given probability. 

## Data Sharing

Data is shared via drive link in /data folder. Contact safakk.bilici.2112@gmail.com if there is a access trouble.





