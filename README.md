# Is Data Augmentation Really Necessary?
Denoise-Then-Classify Approach on CIFAR10

CS 497 Final Project

In the repository, the "models.py" file contains the source code for all the utilized models. The "main.ipynb" notebook provides an easy way to reproduce all experiments. [Link to Github Repository](https://github.com/nemonemonee/denoise-then-classify/tree/main)

### 1. Introduction
Adversarial attacks in machine learning are crafted by introducing a subtle amount of noise, imperceptible to the human eye, but powerful enough to deceive models into making incorrect classifications.  In this project, I want to examin the hypothesize that if the models that are robust to noise can exhibit enhanced resilience in adversarial scenarios, since the source of domain shift made by adversarial examples is just noise.
### 2. Procedures
#### 2.1. Model I: Baseline
This model is designated as the Baseline Classifier. It is trained exclusively with clean images and features an architecture consisting of a Convolutional Neural Network (CNN) followed by a Transformer encoder and a linear output head.
#### 2.2. Model II: Denoise-then-Classify Approach
Model II introduces a denoise-then-classify strategy, training the denoiser on perturbed images while employing the same classification model as used in Model I, aiming to improve robustness against noise. The architecture of the denoiser or noise predictor is a U-net that takes in the noisy images and the perturbation level and outputs the added noise.
#### 2.3. Model III: Dataset Augmentation Approach
The Dataset Augmentation Approach is embodied in Model III, which utilizes a classifier of identical architecture to Model I. This model's training set is expanded to include both original and perturbed image, enhancing its ability to generalize from varied data inputs.
#### 2.4. Model IV: Joint Training Approach
Model IV maintains the architectural design of Model II but diverges in its training methodology by jointly training the denoiser and classifier components, rather than in a sequential manner. This integrated approach serves as an upper bound in comparisons.
#### 2.5 Perturbation Level Adjustment in Training
The perturbation level (t), representing noise variance, is strategically varied to incorporate curriculum learning principles, aiming to enhance model robustness without overfitting to a specific t value. For the denoiser model, t is reduced from 0.3 to 0.05, sharpening its precision with decreasing noise levels. In contrast, for the Data Augmentation and Joint Training approaches, t increases from 0 to 0.3, exposing models to a wider range of noise. This gradual adjustment of t enhances models' robustness to handle various perturbation levels.
#### 2.6 Adversarial Part
During the final project proposal presentation, a compelling observation was made: adding a new model to the forefront and claiming it enhances robustness against adversarial attacks, when these attacks primarily target the latter part of the model, misses the point. In response to this insightful feedback, my strategy is adopted to produce adversarial examples across all four models to conduct a comprehensive comparison. The hypothesis I aim to test is that adversarial examples generated against both the denoise-then-classify and the jointly-trained models will result in significantly more detectable noise.
##### * update *
The adversarial generation method I implemented needs to take the perturbation level as an input. Therefore, I adapted the method here to use the same adversarial attack algorithm with the same perturbation level and compare the success rate at which each output label remains unflipped.
### 3. Results (To be Continued)
#### 3.1 Accuracy On Clean And Noisy Images
![Figure 1](images/fig1.png)

The bar chart presents the accuracy of four different models on the CIFAR10 dataset, comparing performance on both clean and noisy data. The noisy images are constructed using perturbation level t = 0.3.

The Baseline Classifier shows a significant drop in performance with noisy images. It has the highest accuracy on clean data at 63%, but this decreases to 29% with noise, indicating a bad robustness to noise perturbations.

The Denoise-Then-Classify Approach shows an improved robustness to noise with a smaller gap between clean (45%) and noisy (50%) accuracy. 

The Data Augmentation Approach shows a better performance in both clean and noisy images in comparison with the Denoise-then-classify approach.

The joint training approach shows nearly identical performance on the clean and noisy dataset, indicating the best robustness.
#### 3.2 Robustness to Different Nosie Level
![Figure 2](images/fig2.PNG)

As shown in this figure, all three mehtods other than baseline classifier show a consistent performance over different perturbation levels.

### 3.3 Adversarial Part
![Figure 3](images/fig3.PNG)

This is the first 8 images in the test set.
The true labels are cat, ship, ship, plane, frog, frog, car, frog.

![Figure 4](images/fig4.PNG)

This is the adversarial examples generated on the baseline classifier. The perturbation level used here is 0.05 and the noise is not detectable. The other adversarial examples generated on other models basically look the same, so I will only include the pair of original predicted labels and the predicted labels after adversarial attack.

For the baseline classifier:

Orig Pred labels: dog truck truck ship frog frog car frog 

Predicted labels: bird car car ship deer truck dog plane

For the Denoise-Then-Classify Approach:

Orig Pred labels: cat truck ship ship deer cat dog frog 

Predicted labels: frog car truck ship cat cat dog plane

For the Data Augmentation Approach:

Orig Pred labels: cat ship ship plane deer frog dog frog 

Predicted labels: frog car car ship deer frog dog deer

For the Jointly Training Approach:

Orig Pred labels: cat ship ship plane frog bird car frog 

Predicted labels: dog ship truck ship bird deer cat deer

### 4. Analysis
#### 4.1. Robustness to Domain Shft
The jointly training approach serves as the upper bound as expected. Similarly, the performance of the data augmentation method meets expectations: it demonstrates robustness to noise, with results slightly below the upper bound. However, the proposed method falls short of expectations. Its performance, while stable across varying levels of noise, is suboptimal. There are several potential reasons for this failure: firstly, training models separately may not be effective; secondly, predicting noise in low-resolution images could inherently be challenging. It is worth noting that the better performance of the joint training approach might be attributed to the more complex architecture of the model. 

#### 4.2. Adversarial Part
In short, all models perform badly when using the gradient of each model itself to generate adversarial examples.

### 5. Future Work
Given the low resolution of the CIFAR-10 dataset, where distinguishing some images can be challenging even to the human eye, replicating the experiment on a dataset like ImageNet, which has much higher resolution images, would indeed be interesting. This shift might significantly affect the models' performance due to the richer detail and information available in higher-resolution images.

Furthermore, there might be a misalignment between the noisy image classification task and the adversarial scenarios being simulated. Typically, adversarial attacks involve minor perturbations, yet the models have been trained across a broader spectrum of noise levels. Refocusing the training of the denoiser on smaller perturbations may yield results that are more representative of actual adversarial conditions and could improve the model's effectiveness in countering such attacks.

The mixed results from the experiment also raise questions about the successes reported in the paper "Defense against Adversarial Attacks Using High-Level Representation Guided Denoiser" by Liao et al., 2018. Replicating their work could provide valuable insights into the discrepancy in outcomes and potentially highlight subtle nuances that could account for their reported success.
