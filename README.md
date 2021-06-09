# Singapore Hawker Food Classifier
This is my capstone project as part of the General Assembly's Data Science Immersive Course (DSI 21, Singapore).

## Problem Statement
The goal of this project is to build an image classifier for the 12 most famous hawker food in Singapore using convolutional neural network (CNN) and transfer learning. Performance of our models will be evaluated using the accuracy score (i.e. ratio of number of correct predictions to the total number of input images) for our test set (i.e. hold-out set of images to prevent data leakage).  

## Why Hawker Food in Particular?
Singapore's hawker culture is a source of pride for Singaporeans from all walks of life. It is fundamentally a reflection of our heritage and plays an important role in our daily lives. Given its pivotal role in our society, it would be useful to document snippets of Singapore's hawker food culture to ensure its continuity. This is why Ministry of Culture, Community and Youth (MCCY) and the National Heritage Board (NHB) have actually kickstarted a campaign to solicit food images via online photo contributions  ([*Source: Our SG Heritage*](https://www.oursgheritage.gov.sg/ourhawkerculture-online-photo-contributions/)). While the photos contributed are tagged by their individual contributors, our image classifier can serve as a sanity check to weed out instances of inaccurate tagging. This helps to ensure that our documentation efforts are accurate and efficient. 

Furthermore, with the recent successful inscription of Singapore's hawker food culture into the UNESCO Representative List of the Intangible Cultural Heritage of Humanity in December 2020, tourists may be enticed to try out our hawker food when visiting Singapore. However, given its wide variety, they may find it difficult to distinguish between the various classes. Having an image classifier for Singapore's 12 most famous hawker food would thus help them better learn, differentiate and appreciate our hawker food culture.


## Data
Given the lack of available dataset re hawker food, we collected our images via scraping Google Images (see Notebook 1 for codes to do so). In its entirety, we managed to scrape ~ 1400 images per hawker food class for 12 classes ([*Source: Google Drive*](https://drive.google.com/drive/folders/1hFmc5qQ5OGdm57wocz1Xc3yziemFYo6y?usp=sharing)). For the purpose of training, validating and testing our models, we then split our images as follows: 

|  |Training|Validation|Test|
|---|---|---|---|
|Image per class |1,000|200|200|
|Total images|12,000|2,400|2,400

We have deliberately split our images into 3 sets so that the validation set could be used for tuning of hyperparameters while the test set will only be used for the final evaluation re accuracy score to prevent any data leakage.

The 12 classes of food images are as follows:
- Bak Chor Mee
- Char Kway Teow
- Chicken Rice
- Hokkien Prawn Mee
- Kaya Toast and Egg
- Laksa
- Nasi Lemak
- Oyster Omelette
- Roast Meat Rice
- Roti Prata
- Satay
- Wanton Mee

## Model Setup
In building our image classifier, we have predominantly two groups of models:
- Group 1: Own convolutional neural network (CNN) model with its respective layers (e.g. Conv2D, MaxPool2D, Dropout and Dense layers)
- Group 2: Transfer learning using pre-trained models (i.e. MobileNetV2, VGG16 and ResNet50) for feature extraction and prediction

The image below (adapted from [*Medium.com*](https://purnasaigudikandula.medium.com/deep-view-on-transfer-learning-with-iamge-classification-pytorch-5cf963939575)) provides a broad overview of our modeling approach. 

![image](https://user-images.githubusercontent.com/79981889/121153773-f0695e00-c878-11eb-8f0a-63678809c511.png)

## Model Evaluation and Predictions
In developing our image classifier, we built 4 models (CNN, MobileNetV2, VGG16 and ResNet50). As we are primarily concerned with classifying the images into their correct hawker food classes, we decided to use the test accuracy score (i.e. ratio of number of correction predictions to the total number of images from our test (hold-out) set) as our evaluation metric. The accuracy scores for our respective models are as follows: 

|  |Training Accuracy|Validation Accuracy |Test Accuracy|
|---|---|---|---|
|Baseline|---|---|8.3%|
|CNN|77.2%|75.3%|76.4%|
|MobileNetV2|76.8%|74.8%|73.4%|
|VGG16|88.9%|81.1%|80.8%|
|ResNet50|86.1%|82.2%|82.8%|

*Note: In arriving at the best accuracy scores, we also tested these models with different combinations of their hyperparameters (e.g. data augmentation factors, learning rate, loss function* Saved models can be assessed via [*Google Drive*](https://drive.google.com/drive/folders/1K4xtmYleTgM1ivVsSnGDWbPVnG1lkj1r?usp=sharing).


Based on our evaluation metrics, we ultimately decided to go with **ResNet50** as it has the highest test accuracy score of 82.8% (taking into account that all four models have rather similar loss/val loss figures). Of the 2,400 test images, our ResNet model managed to accurately predict ~1,987 images into their respective classes. The diagram below provides a quick breakdown of the predictions by class:

![image](https://user-images.githubusercontent.com/79981889/121157921-8bb00280-c87c-11eb-9017-954f1d708715.png)

From the confusion matrix above, we can observe that our ResNet50 model is generally good at predicting the correct classes since Bak Chor Mee (despite having the lowest accuracy amongst all food classes) has 146/200 images correctly classified.

## Limitations
When analysing the above confusion matrix, we noticed that our ResNet50 model tends to struggle with noodle-related images (e.g. 17 Bak Chor Mee images inaccurately predicted as Laksa or 14 Hokkien Prawn Mee images as Laksa). This phenomenon is also observed in other models that we have built. To overcome this and as a possible extension to our project, we can consider collecting and training our models on even more images so that they get better at recognising and predicting noodle-related images.

## Future Work
Apart from collecting more images (per class) to overcome our limitations, we can also expand the coverage of our image classifier to include more hawker food classes. This will improve the comprehensiveness of our model. 

In terms of practical usage, we can collaborate with MCCY and NHB to deploy our model in their Our Singapore Heritage website to ensure that photo contributions are accurately tagged. To facilitate tourists in distinguishing between the various hawker food classes, we can also work to deploy our image classifer in an app so that they can easily upload images for prediction.
