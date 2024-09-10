# ICIP VIP Cup 2024 - Hybrid Method for OCT Image Classification

This repository contains the code and methodology used by our team in the [ICIP VIP Cup 2024 competition](https://2024.ieeeicip.org/vip-cup/). We developed a hybrid method for classifying Optical Coherence Tomography (OCT) images to identify subjects with Diabetic Macular Edema (DME), Normal, and Non-DME conditions using a multi-instance learning approach.

## Overview

In this project, we worked with a dataset containing OCT B-scans. Each subject in the dataset has between 50 to 300 B-scans. The goal was to classify subjects based on these B-scans. We employed a hybrid vision transformer (ViT) model and a Siamese network with a three-step approach:

1. **Data Preparation**: Splitting the dataset into three distinct datasets (Data 1, Data 2, and Data 3) for training and evaluation.
2. **Hybrid Vision Transformer Training**: Classifying Non-DME subjects using Data 2.
3. **Siamese Network Training**: Differentiating Normal from DME subjects using Data 3.

## Data Preparation

The dataset was divided into three parts:

- **Data 1**: The main dataset containing all subjects and their corresponding B-scans, divided into training and test sets.
  - `NORMAL` (0): 34 subjects
  - `DME` (1): 24 subjects
  - `Non-DME` (2): 23 subjects

- **Data 2**: Used for training a hybrid vision transformer model to classify Non-DME subjects from the Others (Normal + DME).
  - Training Set: Contains 3702 B-scans of Non-DME and 932 B-scans of Others.
  - Test Set: Contains 862 B-scans of Non-DME and 91 B-scans of Others.

- **Data 3**: Used for training a vision transformer-like model in a Siamese network to classify Normal from DME.
  - Each subject had 1-5 B-scans selected that likely contain DME or Normal retinal features.

## Methodology

The workflow of our method consists of the following steps:

1. **Pre-training of Vision Transformer (ViT)**:
   - Train a Vision Transformer (ViT) model on the OCT 2017 dataset (Kermany dataset).
   - Fine-tune this model on **Data 2** to classify Non-DME from Other subjects.

2. **Classification of Non-DME Subjects**:
   - For each subject in **Data 1**, calculate the average probability of B-scan classifications.
   - If a subject has a higher probability of being Non-DME, classify it as such.

3. **Siamese Network Training**:
   - Train a Siamese network with triplet loss on **Data 3** to find the best embeddings for Normal and DME subjects.
   - Convert all B-scans in the "Others" category (Normal + DME) using the trained Siamese network to embedding vectors.

4. **Clustering and Classification**:
   - Use K-means clustering to divide these vectors into two clusters.
   - Assign labels to the clusters based on which cluster contains the majority of Normal or DME B-scans.
   - For remaining subjects, determine their classification based on the percentage of their B-scans in the Normal cluster.

5. **Final Classification**:
   - A subject is classified as Normal or DME based on the clustering results and the distribution of its B-scans.

## Conclusion

This hybrid approach using Vision Transformers and Siamese networks provides a robust method for multi-class classification of OCT images, especially in distinguishing between Non-DME, Normal, and DME subjects.

## Repository Structure

- `models/`: Scripts and code for training the Vision Transformer and Siamese network.
- `results/`: Evaluation metrics and results of the classification.
- `README.md`: This file.


## Acknowledgments

We thank the ICIP VIP Cup 2024 organizers and the Isfahan University of Technology for their support.

