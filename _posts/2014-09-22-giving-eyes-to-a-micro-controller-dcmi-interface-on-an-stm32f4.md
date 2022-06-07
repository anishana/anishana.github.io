---
title: 'Giving eyes to a micro controller: DCMI interface on an STM32F4'
date: 2014-09-22 15:07:44 +05:30
categories:
- Image Processing &amp; Computer Vision
tags:
- DCMI
- Embedded Systems
- Image Processing &amp; Computer Vision
- STM32F4
author: ameya005
comments: true
layout: post
link: https://ameyajoshi005.wordpress.com/2014/09/22/giving-eyes-to-a-micro-controller-dcmi-interface-on-an-stm32f4/
wordpress_id: 275
---

It has been another long hiatus between posts. But, I have managed to learn and do quite a bit of stuff in these last few months and it has been rewarding to say the least.

Recently, I have had to work on an embedded platform for image processing. It was quite a big deal as I had never worked with any sort of embedded platform before and the kind of work is quite different from what I have done before. So, my first task was to interface a camera with a microcontroller. After consulting with my friends, [Shrenik](http://embeddlinux.blogspot.in/) and [Vinod](http://blog.vinu.co.in/), I decided to use a microcontroller which provides a hardware camera interface instead of writing the complete firmware from scratch for an ATMEGA as I had planned on doing earlier.

After some research, I ended up selecting the well known STM32F4 series of microcontrollers. The [STM32F407](http://www.st.com/web/catalog/mmc/FM141/SC1169/SS1577/LN11) is a high powered μC with an ARM Cortex M4 processor running at 168 MHz. The development board available has 1 Mb of onchip flash and 192 kB of SRAM. After playing with GBs of RAM, it sure was tough to be excited about a few kB of SRAM, but it was a different challenge to solve the problem using as few resources as possible. The STM32F407 features a a hardware camera interface known as DCMI ( Digital Camera Interface). It is compatible with a huge range of camera modules on the market.

I had also decided to use the [OV2640 camera module](http://www.uctronics.com/ov2640-2mp-hd-cmos-camera-module-adapter-board-jpeg-out-p-1442.html) as it features an on-board JPEG encoder and is quite well documented. After a few days of familiarizing with the basic concepts and fiddling around with the standard peripherals library from  STM, I came across this amazing implementation, [OpenMV](http://www.google.co.in/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CB4QFjAA&url=http%3A%2F%2Fhackaday.io%2Fproject%2F1313-OpenMV&ei=BuofVNDQH5WVuATc64KgAg&usg=AFQjCNEe8m_KYv2zyseP90eyYpNXlxySdw&sig2=8eqVshIbx1sIroXtI7aIzw&bvm=bv.75775273,d.c2E). I found it extremely helpful to understand the intricacies of image processing on embedded systems.

The primary issues that I faced while working on this project were:



	
  * Understanding DCMI and DMA interfaces of the STM32 controller.

	
  * Understanding the communication and synchronization between the camera and the controller.

	
  * Clocks and STM's unique proposition of allowing us to turn off peripheral clocks when required for low power usage.


I am going to be blogging about my experiences regarding my foray into the world of micro-controllers and camera control soon. Until then, here are a few images that I captured with my setup.

[![test4](http://ameyajoshi005.files.wordpress.com/2014/09/test4.jpg)](https://ameyajoshi005.files.wordpress.com/2014/09/test4.jpg)[![test2](http://ameyajoshi005.files.wordpress.com/2014/09/test2.jpg)](https://ameyajoshi005.files.wordpress.com/2014/09/test2.jpg)
