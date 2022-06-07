---
title: 'SuperPixels: SLIC and more'
date: 2013-12-03 23:32:41 -08:00
permalink: "/posts/2013/12/04/superpixels-slic-and-more/"
categories:
- Image Processing &amp; Computer Vision
tags:
- Computer Vision
- object segmentation
- segmentation
- SLIC
- SLIC Superpixels
author: ameya005
comments: true
layout: post
wordpress_id: 182
---

Hey guys,

I recently came across a very cool computer vision concept - SuperPixels. Superpixels are what you would call aggregations of pixels with common features - for example, color, illumination, absorbance, spatial location etc. It is perhaps the precursor to full object segmentation. Here, I am going to discuss a specific kind of superpixels called SLIC.

SLIC stands for Simple Linear Iterative Clustering. The name defines it all as it is just a simple representation of clustering using k-means. This was first proposed in the following paper:

[Radhakrishna Achanta, Appu Shaji, Kevin Smith, Aure-lien Lucchi, Pascal Fua, and Sabine Susstrunk, SLIC Superpixels, EPFL Technical Report 149300, June 2010.](http://infoscience.epfl.ch/record/149300/files/SLIC_Superpixels_TR_2.pdf)

The basic idea of superpixels is that they contain a lot more high level information as compared to just pixels. Pixels give extremely low level and local information which many-a-times cannot be used without heavy duty processing. Instead, superpixels provide less localized information which makes more sense to have considering problems like segmentation of objects. A simple analogy would be to step back a bit and look at a slightly bigger picture.

To start of with, lets look at a few example images given in the paper:

[![slic1](http://ameyajoshi005.files.wordpress.com/2013/09/slic1.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2013/09/slic1.jpg)

[![slic2](http://ameyajoshi005.files.wordpress.com/2013/09/slic2.jpg?w=200)](http://ameyajoshi005.files.wordpress.com/2013/09/slic2.jpg)


As you can see, pixels are clustered according to their location and color. The different slices represent different sizes of superpixels. One of the major advantages of using SLIC  superpixels is that they show low undersegmentation and high boundary recall. It is one of the most efficient methods in the methods for getting superpixels.


The algorithm is very intuitive. We start off by converting the image to L*a*b* color-space. We then create feature points from each pixel consisting of the L*,a*,b* values and the x and y co-ordinates. These features are then clustered using [k-means clustering](http://en.wikipedia.org/wiki/K-means_clustering)[. ](http://en.wikipedia.org/wiki/K-means_clustering)

Now, one of the brilliant things of SLIC is that it takes the x-y co-ordinates into the picture while clustering. So, you get uniform and well defines superpixels. This can then be further used for segmentation. The distance function used for clustering is given by

[![](http://latex.codecogs.com/png.latex?d_{lab}&space;=&space;\sqrt{&space;(l_{k}&space;-&space;l_{i})^{2}&space;&plus;&space;(a_{k}&space;-&space;a_{i})^{2}&space;&plus;(b_{k}&space;-b_{i})^{2}&space;})](http://www.codecogs.com/eqnedit.php?latex=d_{lab}&space;=&space;\sqrt{&space;(l_{k}&space;-&space;l_{i})^{2}&space;&plus;&space;(a_{k}&space;-&space;a_{i})^{2}&space;&plus;(b_{k}&space;-b_{i})^{2}&space;})

[![](http://latex.codecogs.com/png.latex?d_{xy}&space;=&space;\sqrt{&space;(x_{k}&space;-&space;x_{i})^{2}&space;&plus;&space;(y_{k}&space;-&space;y_{i})^{2}&space;})](http://www.codecogs.com/eqnedit.php?latex=d_{xy}&space;=&space;\sqrt{&space;(x_{k}&space;-&space;x_{i})^{2}&space;&plus;&space;(y_{k}&space;-&space;y_{i})^{2}&space;})

[![](http://latex.codecogs.com/png.latex?D_{S}&space;=&space;d_{lab}&space;&plus;&space;\frac{m}{S}d_{xy})](http://www.codecogs.com/eqnedit.php?latex=D_{S}&space;=&space;d_{lab}&space;&plus;&space;\frac{m}{S}d_{xy})

So, we start off with K equally spaced clusters all across the image. These clusters are centred around the lowest gradient position in a 3x3 neighbourhood around the equally spaced points. We do this to avoid putting them at edges and to reduce the probability of choosing a noisy pixel. We calculate the image gradients as follows:

![G(x,y) =  \| I(x+1,y) - I(x-1,y) \|^2 + \|I(x,y+1) - I(x-1,y)\|^2](http://www.sciweavers.org/tex2img.php?eq=G%28x%2Cy%29%20%3D%20%20%5C%7C%20I%28x%2B1%2Cy%29%20-%20I%28x-1%2Cy%29%20%5C%7C%5E2%20%2B%20%5C%7CI%28x%2Cy%2B1%29%20-%20I%28x-1%2Cy%29%5C%7C%5E2&bc=White&fc=Black&im=jpg&fs=12&ff=modern&edit=0)

where $latex I (x,y)$ is the lab vector corresponding to the pixel and $latex \|.\|$ is the L2 norm.
Each pixel in the image is then associated with the nearest cluster centre in the $latex labxy$ feature space. After all pixels are associated with one or the other cluster, we again calculate a new cluster centre as the average value of the $latex labxy$ features in each cluster. We then repeat the process of clustering and recalculating until we achieve convergence.

As a result, we will have nicely clustered groups of pixels, both in color as well as spatial domain. Now, there might be a few stray pixels that may have got clustered in the wrong clusters. Though this is pretty rare according to the authors of the paper and my experiments, we enforce a connectivity constraint on them in order to remove these outliers. The paper doesnot mention this in any great detail, but connectivity is an important condition to impose, especially if we are going to use SLIC superpixels for segmentation.

Thats it for the explanation of an amazing technique, folks! I will post my OpenCV code link in a few days as well as will discuss a few issues with the code and segmentation techniques using these in the next few posts.

P.S.:By the way, we will also be looking at Bag of Words in a little more detail in a few more days. Be on the look out for a barrage of posts.
