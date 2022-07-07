# Project 4 – Classification of white matter lesions in lupus

# Introduction

This project was developed in the context of the activities of the graduate course [_Science and Visualization of Health Data_](https://ds4h.org/), offered in the first semester of 2022, at Unicamp.

## Members

<div align="center">
  
Name | RA | Speciality
---|---|---
Luan de Oliveira Silveira | 204099 | Computer Science
Wilson Bagni Junior | 010097 | Computer Science
  
</div>

## Introduction

## Methodology 

We are provided a folder with labeled medical images and a respective mask to extract the lesion. The mask select a Region of Interest (ROI) in the image.
The images were separated by patient and then in two groups (Train and Validation). The images were splitted in a way that there wasn't images from the same patient in the two groups at same time.

The images with mask applied were preprocessed using [Min-max normalization](https://en.wikipedia.org/wiki/Feature_scaling#Rescaling_(min-max_normalization)) and the feature [kurtosis](https://en.wikipedia.org/wiki/Kurtosis) was extracted of each them.

Then using a [Support Vector Machine (SVM)](https://en.wikipedia.org/wiki/Support-vector_machine) classifier, a model was trained to classify the images between ischemic (AVC) and demyelinating lesions(EM).

The images from patients with Systemic Lupus Erythematosus (SLE) were processed in the same way and then the SVM classied then into SVM ou EM type.

The code used to process the images and develop the SVM is avaliable in the site: XXXXXXXXXXXXXXX. 

Note: Because the limitation of space in github, the images were not provide in this platform.


## Visual Analysis 

Before present the SVM's result, It is worth analysing the images and some features.

xXXXXXX

## Results


## Conclusions




