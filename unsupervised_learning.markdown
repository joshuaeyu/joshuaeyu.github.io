---
layout: default
title: Joshua E. Yu - Clustering and Dimensional Reduction
permalink: /unsupervised_learning/
---

# Augmenting Neural Network Learning with Clustering and Dimensional Reduction

Updated January 29th, 2025

<hr>{:.weak-hr}

1. [Overview](#overview)
2. [Datasets](#datasets)
3. [Clustering](#clustering)
4. [Dimensionality Reduction](#dimensionality-reduction)

<hr>{:.weak-hr}

## Overview

Unsupervised learning encompasses a wide range of techniques which aim to draw conclusions from unlabeled data, and each technique can have many approaches in itself. 

 - **Clustering** is a technique which attempts to partition data into groupings based on some notion of similarity; some clustering approaches include hierarchical clustering, *k*-means clustering, and Gaussian mixture models (GMM). 

 - **Dimensionality reduction** attempts to transform a set of features into a more simplified representation while preserving as much of the underlying signals in the original data as possible. Some approaches include random projection, principal component analysis (PCA), and independent component analysis (ICA).

On this page, I will summarize my work and findings for the Unsupervised Learning assignment in Georgia Tech's CS 7641 course and discuss how clustering and dimensionality reduction can be useful for optimizing supervised learning. I chose to explore the [Dry Bean](https://archive.ics.uci.edu/dataset/602/dry+bean+dataset){:target="_blank"} and [Estimation of Obesity Levels Based On Eating Habits and Physical Condition](https://archive.ics.uci.edu/dataset/544/estimation+of+obesity+levels+based+on+eating+habits+and+physical+condition){:target="_blank"} ("Obesity") datasets from the [UC Irvine Machine Learning Repository](https://archive.ics.uci.edu){:target="_blank"}. 

*(****Note:*** *Due to Georgia Institute of Technology's CS 7641 course policy, the GitHub repository for this work is private and available upon request.)*

## Datasets

The Dry Bean dataset is made up of continuous features extracted by a computer vision system from images of dry beans, and each instance is labeled with the varietal of dry bean. Supervised learning techniques are successful at learning this dataset—I achieved a test F1 score of 0.93 using scikit-learn's [MLPClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html).

The Obesity dataset is made of continuous and categorical features derived from original and synthetic survey data, and each instance is labeled with the obesity level of the individual. Similarly, supervised learning of this dataset is successful—I achieved a test F1 score of 0.95 using scikit-learn's MLPClassifier.

## Clustering

Clustering on its own is useful for deriving meaningful groupings of instances in an unlabeled dataset. In addition, as I'll show here, it can be useful as a preprocessing stage for supervised learning of labeled datasets.

First, let's visualize the effectiveness of clustering on the datasets (contingency matrices are also a useful tool but I did not include them here for brevity). For this, I used scikit-learn's [GaussianMixture](https://scikit-learn.org/stable/modules/generated/sklearn.mixture.GaussianMixture.html) and [KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) The number of clusters for each algorithm and dataset were selected by analyzing the Calinski-Harabasz index and Davis-Bouldin index at different values of *k*.

!["Dry Bean dataset clustering scatter plot"](/graphics/p1_drybean_clustering_scat.png "Dry Bean dataset clustering scatter plot"){:.project-image}
!["Obesity dataset clustering scatter plot"](/graphics/p1_obesity_clustering_scat.png "Obesity dataset clustering scatter plot"){:.project-image}

Both GMM (via expectation-maximization or EM) and *k*-means clustering are much more effective on Dry Bean than Obesity. For Dry Bean, nearly all true classes in the dataset are partitioned, with the exception of true classes 0 and 2, which are grouped together. Only true class 6 can really be discerned in Obesity. These results are likely because Dry Bean's features are continuous and based on geometric properties, whereas Obesity's features are noisier and based on human survey responses.

So how can clusters be used to optimize supervised learning? The approach I explored was to **use the labels generated through clustering as a *new input* to the corresponding neural network**. With Dry Bean, this meant adding a 17th feature to its existing 16 features. Obesity also happens to contain 16 features, to which I added a 17th.

Using five-fold cross validation, I performed a grid search to optimize the network architecture of the new, augmented neural network models. I then tested the models on the 20% of the data I had held out for testing, yieldling the following results:

!["Neural network performance table"](/graphics/neuralnetwork_dimred_clustering.png "Clustering scatter plot for Dry Bean"){:.project-image-large}

I was happy to find that **the optimal architectures of the augmented models were much simpler than those of the original models!** For Dry Bean, the number of hidden layers dropped from three to just one, and for Obesity, the number of hidden units in its single hidden layer dropped from 100 to just 9 and 7. 

Additionally, **the fit times of the augmented models were shorter than those of the original models!** For Dry Bean, fit time decreased by up to 39%, and for Obesity, it decreased by up to 35%.

However, F1 score tells us that not all datasets are made equal. As shown earlier, clustering was successful on Dry Bean but not on Obesity (where many true classes were misconstrued). It turns out that supervised learning using Obesity's wildly inaccurate cluster data truly does impact prediction performance—the F1 scores of the augmented Obesity models were up to 27% worse. In contrast, Dry Bean's mostly accurate cluster data did *not* decrease its F1 score at all! From this, I concluded that **the Obesity dataset's low-quality unsupervised cluster labels negatively impacted the prediction performance of its augmented supervised learning models.**


## Dimensionality Reduction

(to be completed)



Thanks for reading! Feel free to reach out on LinkedIn if you have questions or concerns. :)