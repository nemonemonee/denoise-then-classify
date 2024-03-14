# Is Data Augmentation Really Necessary?
Denoise-Then-Classify Approach on CIFAR10

CS 497 Final Project

In the repository, the "models.py" file contains the source code for all the utilized models. The "main.ipynb" notebook provides an easy way to reproduce all experiments. Additionally, a PDF document is produced from the TeX code.

### 1. Introduction

### 2. Procedures
#### Model I: Baseline
This model is designated as the Baseline Classifier. It is trained exclusively with clean images and features an architecture consisting of a Convolutional Neural Network (CNN) followed by a Transformer encoder and a linear output head.
Model II: Denoise-then-Classify Approach
Model II introduces a denoise-then-classify strategy, training the denoiser on perturbed images while employing the same classification model as used in Model I, aiming to improve robustness against noise. The architecture of the denoiser or noise predictor is a U-net that takes in the noisy images and the perturbation level and outputs the added noise.
Model III: Dataset Augmentation Approach
The Dataset Augmentation Approach is embodied in Model III, which utilizes a classifier of identical architecture to Model I. This model's training set is expanded to include both original and perturbed image, enhancing its ability to generalize from varied data inputs.
Model IV: Joint Training Approach
Model IV maintains the architectural design of Model II but diverges in its training methodology by jointly training the denoiser and classifier components, rather than in a sequential manner. This integrated approach serves as an upper bound in comparisons.


The perturbation level (t), representing noise variance, is strategically varied to incorporate curriculum learning principles, aiming to enhance model robustness without overfitting to a specific t value. For the denoiser model, t is reduced from 0.3 to 0.05, sharpening its precision with decreasing noise levels. In contrast, for the Data Augmentation and Joint Training approaches, t increases from 0 to 0.3, exposing models to a wider range of noise. This gradual adjustment of t enhances models' ability to handle various perturbation levels

### 3. Results ()

### 4. Analysis

### 5. Conclusion and Future Work

