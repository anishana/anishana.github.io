---
title: VLAD- An extension of Bag of Words
date: 2014-03-29 22:26:00 +05:30
categories:
- Image Processing &amp; Computer Vision
tags:
- Bag of Features
- bag of words
- Computer Vision
- Image Classification
- Machine Learning
- Object Recognition
- scene classification
- VLAD
author: ameya005
comments: true
layout: post
wordpress_id: 220
---

Recently, I was a participant at TagMe- an image categorization competition conducted by Microsoft and Indian Institute of Science, Bangalore. The problem statement was to classify a set of given images into five classes: faces, shoes, flowers, buildings and vehicles. As it goes, it is not a trivial problem to solve. So, I decided to attempt my existing bag-of-words algorithm on that. It worked to an extent, I got an accuracy of 86% approximately with SIFT features and an RBF SVM for classification. In order to improve my score though, I decided to look at better methods of feature quantization. I had been looking at VLAD (Vector of Locally Aggregated Descriptors): A first order extension to BoW for my Leaf Recognition project.

So, I decided to attempt to use VLAD using OpenCV and implemented a small function based on the BoW API currently in OpenCV for VLAD. The results showed remarkable improvement with an accuracy of 96.5 % using SURF descriptors on teh validation dataset provided by the organizers.


### **What is VLAD**


Recalling BoW, it involved simply counting the no. of descriptors associated with each cluster in a codebook(vocabulary) and creating a histogram for each set of descriptors from an image, thus representing the information in a an image in a compact vector. VLAD is an extension of this concept. We accumulate the residual of each descriptor with respect to its assigned cluster. In simpler terms, we match a descriptor to its closest cluster, then for each cluster, we store the sum of the differences of the descriptors assigned to the cluster and the centroid of the cluster. Let us have a look at the math behind VLAD..


### **Mathematical Formulation**


As with bag of words, we first train a codebook from the descriptors from our training dataset, as $latex C=\{c_1,c_2,...c_k\}$ where $latex k$ is the no. of clusters in K-means. We then associate each $latex d$-dimensional local descriptor, $latex x$ from an image with its nearest neighbour in the codebook.

The idea behind VLAD feature quantization is that, for each cluster centroid, $latex c_i$, we accumulate the difference $latex x-c_i$ where for each $latex x$, $latex c_i = NN(x)$

Representing the VLAD vector for each image by $latex v$, we have,

$latex v_{ij} =\sum_{x|x=NN(c_i)} {(x_j - c_{ij})}$

where $latex i=1,2,3...k$ and $latex j=1,2,3..d$

The vector $latex v$ is subsequently normalized with its $latex L_2$ norm as $latex v=\frac{v}{\|v\|_2}$


### **Comparison with BoW**


The primary advantage of VLAD over BoW is that we add more discriminative property in our feature vector by adding the difference of each descriptor from the mean in its voronoi cell. This first order statistic adds more information in our feature vector and hence gives us better discrimination for our classifier. This also points us to other improvements we can   adding higher order statistics to our feature encoding as well as looking at soft assignment,i,e. assigning each descriptor multiple centroids weighed by their distance from the descriptor.


### **Experiments**


Here are a few of my results on the TagMe dataset.

[![results](http://ameyajoshi005.files.wordpress.com/2014/03/results.jpg?w=620)](http://ameyajoshi005.files.wordpress.com/2014/03/results.jpg)


### 




### 




### 




### 




### Improvements to VLAD:


There are several extension possible for VLAD, primarily various normalization options. Arandjelov and Zissermann in their paper, A[ll about VLAD,](http://www.robots.ox.ac.uk/~vgg/publications/2013/arandjelovic13/?update=1) propose several normalization techniques, including intra normalization and power normalization alonging with a spatial extension - MultiVLAD. [Delhumeau et al](http://delivery.acm.org/10.1145/2510000/2502171/p653-delhumeau.pdf?ip=115.248.130.148&id=2502171&acc=ACTIVE%20SERVICE&key=045416EF4DDA69D9.B8A9898014426BF2.4D4702B0C3E38B35.4D4702B0C3E38B35&CFID=309703199&CFTOKEN=17981182&__acm__=1396111162_07a913959d90189257582343022a03ff), propose several different normalization techniques as well as a modification to the VLAD pipeline to show improvements to almost state of the art.

Other references also stress on spatial pooling i.e. dividing your image into regions to get multiple VLAD vectors for each tile to better represent local features and spatial structure. A few also advise soft assignment, which refers to assignment of descriptors to multiple clusters, weighed by their distance from the cluster.


### Code:


Here is a link to my code for TagMe. It was a quick has job for testing so it is not very clean though I am going to clean it up soon.

[**https://github.com/ameya005/VLAD-Implementation**](https://github.com/ameya005/VLAD-Implementation)

also, a few references for those who want to read the papers I referred:

1.[Jégou, H., Perronnin, F., Douze, M., Sánchez, J., Pérez, P., & Schmid, C. (2012). Aggregating local image descriptors into compact codes. _Pattern Analysis and Machine Intelligence, IEEE Transactions on_, _34_(9), 1704-1716.](http://hal.archives-ouvertes.fr/docs/00/54/86/37/PDF/jegou_compactimagerepresentation.pdf)

2. [Delhumeau, J., Gosselin, P. H., Jégou, H., & Pérez, P. (2013, October). Revisiting the VLAD image representation. In _Proceedings of the 21st ACM international conference on Multimedia_ (pp. 653-656). ACM.](http://dl.acm.org/citation.cfm?id=2502171)

3. [Arandjelovic, R., & Zisserman, A. (2013, June). All about VLAD. In _Computer Vision and Pattern Recognition (CVPR), 2013 IEEE Conference on_ (pp. 1578-1585). IEEE.](http://courses.cs.washington.edu/courses/cse590v/13au/arandjelovic13.pdf)

Well, that concludes this post. Be on the lookout for more on image retrieval - improvements to VLAD and Fisher Vectors.


