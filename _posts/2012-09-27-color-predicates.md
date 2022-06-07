---
title: Color Predicates
date: 2012-09-27 12:17:09 -07:00
permalink: "/posts/2012/09/28/color-predicates/"
categories:
- Image Processing &amp; Computer Vision
tags:
- human computer interaction
- programming
author: ameya005
comments: true
wordpress_id: 15
---

It's been a long time since I posted something here. But here it is.

I started my current project with a simple objective- I wanted to build a completely natural human- computer interaction system. Basically, my final aim is to just have a few cameras and microphones work as the input devices instead of a mouse and keyboard. So, with this in mind, I started out with my first objective- to segment out and track the hand in a video stream.
The first problem that I encountered was that skin as such is not a single color but a heady mixture of reds, yellows and white.The problem is simplified a bit in the HSV colorspace, but not enough. A simple thresholding program will not work at all given such few constraints. So, while researching on how to solve this problem, I got a paper mentioning color predicates.

Now, what are color predicates? Color predicates, very simply put are a map in the colorspace. A simple region within which every color included is taken as being part of the colors to be segmented and outside of which are the rejected colors. So, I decided that this was the way to go. Took me a couple of days but I was able to build a simple color predicate training program.

My way of implementing color predicates:

I used a matrix of 255x255 to represent the Hue and Saturation values possible. I then provided several images of my hand cropped onto a white background ( Used GIMP - awesome piece of software ). Then I scanned each image pixel by pixel and rejecting white, I simply incremented the value at each H,S index of the matrix if I found a color other than white. Now using this simple algorithm, I trained a color predicate for around 20 images taken in various illumination schemes.  With a few manipulations for rejecting low values, I then used the color predicate for simple thresholding of images in my video stream.

[![](http://ameyajoshi005.files.wordpress.com/2012/09/handtrain2.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2012/09/handtrain2.jpg)             [![](http://ameyajoshi005.files.wordpress.com/2012/09/handtrain3.jpg?w=300)](http://ameyajoshi005.files.wordpress.com/2012/09/handtrain3.jpg)

The best part of this exercise was when I looked at the HSV map thus created in a text editor....amazingly, there were two different regions which could be clearly seen, corresponding to the red spectrum and the yellow spectrum.

The code for the above exercise:

[sourcecode language="cpp"]

//Color Predicates ver 1.2

#include<cv.h>
#include<highgui.h>
#include<stdio.h>

int colorp[256][256];
IplImage *img, *imghsv;
int h,s,v;
int main()
{

int i,j;
for(i=0;i<256;i++)
{
for(j=0;j<256;j++)
{

colorp[i][j]=0;
}
}
FILE *fp = fopen("cp2.bin", "r");
for(i=0;i<256;i++)
fread(colorp[i], sizeof(int), 256, fp);

CvScalar Data;
img=cvLoadImage("train8.jpg");
imghsv=cvCreateImage(cvGetSize(img),8,3);
cvCvtColor(img,imghsv,CV_BGR2HSV);
cvNamedWindow("Image");
cvNamedWindow("HSV");

for(i=0;i<img->height;i++)
{
for(j=0;j<img->width;j++)
{
Data=cvGet2D(imghsv,i,j);
h=Data.val[0];
s=Data.val[1];
v=Data.val[2];
if(s!=0)
{
colorp[h][s]+=1;
}

}
}
fclose(fp);
FILE* fp2=fopen("cp2.bin","w");
for(i=0;i<256;i++)
{
fwrite(colorp[i], sizeof(int), 256*256, fp2);
}

fclose(fp2);

return 0;

}
[/sourcecode]

In the next post, I will be describing my attempt to use the created color predicate.
