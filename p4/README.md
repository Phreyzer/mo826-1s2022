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

 <\div> 
## Results

## Conclusions




