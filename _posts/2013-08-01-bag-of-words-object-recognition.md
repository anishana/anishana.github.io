---
title: 'Bag of Words - Object Recognition '
date: 2013-08-01 00:41:00 -07:00
permalink: "/posts/2013/08/01/bag-of-words-object-recognition/"
categories:
- Image Processing &amp; Computer Vision
tags:
- Bag of Features
- Computer Vision
- Machine Learning
- object classification
- Object Recognition
- SIFT
author: ameya005
comments: true
layout: post
wordpress_id: 170
---

Hey guys,

Its been a really long time since my last post. But this series of posts is going to be a really cool I hope.

Today we are going to discuss one of the most important problems in Computer Vision- **Object Recognition**. We humans tend to trivially recognize objects without consciously paying attention to the fact or even wondering how exactly do we achieve this. You look at a baseball flying towards your face, you recognize it as a baseball about to break your nose, and you duck! All in a matter of a few microseconds.

But the process that your brain undertakes in those few microseconds has eluded perfect implementation in computation for several years now. Object recognition is perhaps, rightly considered the primary problem in computer vision. But recent research advances have made strides in this matter.

I recently undertook a project in which I had to classify leaves into species they come from. And as it sounds, it's not really a trivial problem. It took me a few days to figure out the first steps to such a process. And to start of with I decided to use the Bag-of Words model, a highly cited method for scene and object classification for the above problem.

To begin with, I found a really nice dataset to work with here: http://flavia.sourceforge.net/ . The dataset contains images for 32 species of leaves on plain white backgrounds which simplified my experiment. I am really grateful to them for providing such a comprehensive dataset for free on the web. (Kinda all for [Open Access](http://en.wikipedia.org/wiki/Open_access) now.).

Bag of words is a basically a simplified representation of an image. Its actually a concept taken form Natural Language Processing where you represent documents as an unordered collection of words disregarding grammar. Translating this into CV jargon, it means that we simplify images by picking out features from an image and representing it as a collection of features. A good explanation of what features are can be found at my friend, Siddharth's blog [here.](http://algorithmicthoughts.wordpress.com/2013/01/19/computer-vision-feature-detection/)

To get more technical about the BoW- we construct a vocabulary of features. We then use this vocabulary to create histograms from features for each image and then use a simple machine learning algorithm like SVM or Naive Bayes for classification.

This is the algorithm I followed for BoW. I got a lot of help from Roy's blog [here.](http://www.morethantechnical.com/2011/08/25/a-simple-object-classifier-with-bag-of-words-using-opencv-2-3-w-code/)

1. We pick out features from all the images in our training dataset. I used SIFT (Scale Invariant Feature Transform).

2. We cluster these features using any clustering algorithm. I used K-Means. (Pretty fast in OpenCV)

3. We use the cluster as a vocabulary to construct histograms. We simply count the no. of features from each image belonging to each cluster. Then we normalize the histograms by dividing it with the no. of features. Therefore, each image in the dataset is represented by one histogram.

4. Then these histograms are passed to an SVM for training. I currently use a Radial Basis function multi-class SVM in OpenCV. Using OpenCV's CvSVM::train_auto() function, we get parameters for the SVM using cross validation.

Now why does Bag of Words work? Why use it rather than simple feature matching? The answer to that question is simple: features provide just local information. Using the bag-of-words model we create a global representation of an object. Thus, we take a group of features, create a representation of the image in a simpler form and classify it.

That was for the pros of the algorithm. But there are a few cons associated with this model.

1. As evident, we cannot localize an object in an image using this model. That is to say, the problem of finding where the object of interest lies is still open and needs other methods.

2. We neglect grammar. In CV terms, it means we neglect the position of features relative to each other. Thus the concept of a global shape maybe lost.

As for our Leaf Recognizer, we are still working on improving the accuracy. We are almost at our goal! The following are some of the images we got as a result of the above algorithm:

[![Test_image_1236](http://ameyajoshi005.files.wordpress.com/2013/08/1236.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2013/08/1236.jpg)[![test_image_1242](http://ameyajoshi005.files.wordpress.com/2013/08/1242.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2013/08/1242.jpg)

[![Test_image_3176](http://ameyajoshi005.files.wordpress.com/2013/08/3176.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2013/08/3176.jpg)[![Test_image_3177](http://ameyajoshi005.files.wordpress.com/2013/08/3177.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2013/08/3177.jpg)[![test_image_1373](http://ameyajoshi005.files.wordpress.com/2013/08/1373.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2013/08/1373.jpg)[![test_image_1378](http://ameyajoshi005.files.wordpress.com/2013/08/1378.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2013/08/1378.jpg)
