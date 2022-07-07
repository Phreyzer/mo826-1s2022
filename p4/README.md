# Project 4 – Classification of white matter lesions in lupus

# Introduction

This project was developed in the context of the activities of the graduate course [_Science and Visualization of Health Data_](https://ds4h.org/), offered in the first semester of 2022, at Unicamp.

## Members


<div align="center">
	
	
Nome | RA | Especialização
----|--|-----
Luan de Oliveira Silveira | 204099 | Ciência da Computação
Wilson Bagni Junior | 010097 | Ciência da Computação
</div>

## Introduction

Brain lesions are a type of damage to the brain that may be caused, among other things, by three main conditions:

1. Stroke - A vascular injury or cut of blood supply to the brain. It's perhaps the main cause of brain injuries.
2. Multiple Sclerosis - A condition where lesions appear in multiple areas of the brain.
3. Lupus - An autoimmune disease with multiple manifestations and that can cause brain lesions.

A common way of diagnosing these types of lesions is through imaging of the brain, such as computed tomography (CT) and magnetic resonance imaging (MRI). These images are then examined by a doctor who will classify the type of lesion. With this approach in mind, our first proposal is to train a machine learning model to learn how distinguish between brain lesions caused by stroke and multiple sclerosis, evaluate the performance and relevant attributes being used in the decision making with the hope of a better medical insight into these brain lesions. The second step consists in using the trained model to classify lupus lesions into either a stroke or multiple sclerosis, with the objective of finding similarities that these types of lesions may share.

## Methodology 

We were given an initial folder with medical images of brain lesions caused by two conditions, stroke (AVC) and multiple sclerosis (EM). Our first goal consists in using this dataset to training a svm model to classify a brain lesion into these two classes. We begin by taking the complete collection of patients and randomly splitting them in a 80-20 proportion for the training and test sets (the split was made by patient and not by image to avoid the same patient being used in both the training and test sets). With both sets defined we need to decide on a image normalization approach, for this we trained 100 models with each method and display the statistics of the accuracy in the table below.

<div align="center">
  
Method|Min|Max|Mean|Std
-|-|-|-|-
Normal|60.869565|97.991968|90.281728|6.087545
MinMax|64.5283082|98.734177|89.771374|5.069562
DS|44.758065|96.511628|83.148129|13.452082
ZS|45.588235|94.809689|79.300250|13.104043
Tanh|48.571429|94.901961|81.477037|11.744619
 </div> 
 
As we can see the no normalization method had a better result since the mean accuracy is the best with 90.28% and it doesn't drop below 60% precision with a relatively low standard deviation. We decided to move foward with the no normalization.

Our next step is to compare each feature to determine the best attributes to the model. We do this also running 100 models with a single different attribute and measuring the minimum, maximum, standard deviation and mean accuracy scores.

<div align="center">

Attribute|Mean|Min|Max|Std
-|-|-|-|-
Kurtosis|91.31|80.00|99.62|4.26
Skewness|91.06|77.21|99.64|4.80
Entropy|86.63|76.89|96.51|4.14
Max|85.49|41.98|96.08|6.07
Mean|85.27|35.22|95.30|6.78
Std|85.00|37.86|95.88|8.18
Correlation|83.90|76.81|90.53|2.94
Contrast|79.61|67.98|88.69|4.80
Dissimilarity|76.89|66.09|86.76|4.40
ASM|75.15|51.97|85.42|4.80
Energy|74.77|61.90|86.36|3.99
Homogeinity|72.31|58.97|82.03|4.05
</div>

The kurtosis showed the best performance with a 91.31% mean accuracy score and never dropping below 80%. After also trying a combination of the top performers we arrive at the following result.

<div align="center">
	
Combination|Mean|Min|Max|Std
-|-|-|-|-
Skewness,Kurtosis,Entropy,Max,Mean,Std|91.15|80.21|99.66|4.46
Skewness,Kurtosis,Entropy|90.84|74.62|98.98|4.67
Skewness,Kurtosis|90.50|77.99|99.59|4.54
</div>

As the table above shows there was no real gain from when we used a single attribute. Thus, We will then attempt to train the final svm model only with the kurtosis for simplicity and performance.

The kurtosis of a distribution is commonly interpreted as being related to the shape of the curve. Matematically, its given by the equation <img src="https://render.githubusercontent.com/render/math?math=k=\mu/\sigma^4< $k = \mu_4/\sigma^4$, where $\mu_4$ is the fourth moment of the distribution and $\sigma$ is the standard deviation. Distributions with a negative excess kurtosis, that is, kurtosis minus 3, are called platykurtic and tends to have a tinner tail, meaning that it produces fewer extreme outliers. On the other hand, distributions with a positive kurtosis are known as leptokurtic and have a "fatter tail", producing more extreme outliers. The normal distribution is called mesokurtic and have a excess kurtosis of 0.

<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p4/assets/image01.jpg?raw=true" width="300" height="200">
</div>


The dataset was again ramdomly split into a 80-20 proportion of patients for training and test, the kurtosis was extracted from the image of the lesion (that is, we applyied the mask into the image to obtain only the lesion, and also throwed out the zero pixels with the idea that the model should only be exposed to information of the lesions themselves to be able to distinguish them) and normalized before being given to the classifier.

The final svm had a accuracy score of 91.2% on the testing set, and will be used to classify lesion types from lupus into stroke or multiple sclerosis.

## Results

The first thing to notice is that each given patient has multiple images, and the svm will attempt to classify each image separately, so it's possible that the model will diagnose the same patient with different lesion types depending on the given flair. This is not a problem since our focus here is to study the criteria that it uses to decide on a diagnosis, and looking at different flairs with different diagnosis of the same patient might give us an insight into what the model considers relevant in classifying.

The model used was training with the original 581 images from the 50 stroke patients and the 630 images from the 51 multiple sclerosis ones. No image normalization method was used since it proved inefficient in our previous study. However, the attributes were normalized using the z-score method so its values are closer distributed.

<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p4/assets/image02.png?raw=true" width="375" height="248">
</div>

As we can see from the histogram above the model classified around 55% of the images as multiple sclerosis and 45% as stroke. Lets display the flairs for 4 different patients and the corresponding svm classification label and kurtosis value.

_Patient: 601_

_Lesion: Lupus_

<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p4/assets/image03.png?raw=true" width="684" height="205">
</div>

------------------------------------------------------------------------------------------
_Patient: 604_

_Lesion: Lupus_

<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p4/assets/image04.png?raw=true" width="684" height="205">
</div>

------------------------------------------------------------------------------------------
_Patient: 616_

_Lesion: Lupus_

<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p4/assets/image05.png?raw=true" width="684" height="205">
</div>

------------------------------------------------------------------------------------------
_Patient: 621_

_Lesion: Lupus_

<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p4/assets/image06.png?raw=true" width="684" height="205">
</div>

The images suggest that a frontier exists around a kurtosis value of 10, where an image with a value higher than 10 gets classified as a stroke lesion and a lower value as multiple sclerosis. To confirm our hipotesis lets plot the distribution of kurtosis values for each lesion type.

<div align="center">
<img src="https://github.com/Phreyzer/mo826-1s2022/blob/main/p4/assets/image07.png?raw=true" width="716" height="277">
</div>

As we suspected the kurtosis values for multiple sclerosis (EM) images are more heavily distributed closer to 0 and the kurtosis for stroke (AVC) tends to have a higher value. When attempting to classify the lupus images we can assume that the svm has learned to diagnose as stroke (AVC) images with a kurtosis higher than a certain threshold and as multiple sclerosis below this value.


## Conclusions

Evaluating the performance of the svm classifier using only the kurtosis as attribute, we got on average above 90% accuracy, suggesting that this attribute was a good choice for this problem. Observing the meaning of kurtosis in a distribution we can infer that on stroke (AVC) images the pixels are more narrowly located in a specific range, with a fewer pixels outside these values. In contrast, the relative low values for the kurtosis on multiple sclerosis (EM) suggests that these kind of lesion images have a wider range of pixel values. We can go further and hypothesise that brain lesions caused by a stroke creates a more homogeneous scar tissue while multiple sclerosis is more likely to cause a more heterogeneous lesion.

It is also important to note the possibility of this result being a reflection of a possible bias from the dataset, where one class of lesion naturally darker or lighter than the other.


