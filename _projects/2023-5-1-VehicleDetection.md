---
title: "Multiple Vehicle Detection Utilizing Deep Learning Algorithms"
collection: projects
type: #"northeastern"
permalink: /projects/vehicle-detection/ 
# date: 2023-5-8
period: May 2023 - June 2023
location: 
classes: wide
excerpt: "Trained and tested deep learning based object detection models, RetinaNet and YOLOv5, on an on-road vehicle dataset and their performance was investigated."
---
Some of the content in this page is based on the following report: 
<a style="text-decoration: none;" href="/assets/projects/vehicle-detection/ML for Computer Vision EECE 5644 Final Report.pdf" target="_blank">Report <i class="fa fa-file"></i></a>

# Introduction
The principle and trend of deep learning based object detection models were reserached. Between the several models, two (RetinaNet and YOLOv5) were selected to be tested. In order to investigate the application of object detection in practice, a preprocessed on-road vehicle dataset was prepared. The dataset provided location and vehicle information of vehicle objects in each image. Accordingly, the object detection models were trained, validated, and tested. The resulting performace of each model was analyzed and presented at the end.

# Overview of Object Detection
The task of object detection was to identify and localize objects within digital images or video frames which were basically a sequence of digital images. In order to detect multiple objects in one image or video frame, a training dataset had to provide both classification and localization information. Then, a object detection model trained with the dataset could properly detect where and what they were.

Object localization could be divided into two steps [[1]](#1). The first step was to generate a set of potential object proposals or candidate bounding boxes through a region proposal network (RPN). The proposals acted as potential regions of interest (ROIs) that might contain objects. In the next step, bounding box coordinates of the proposals were refined to precisely fit the objects. It was achieved by bounding box regression. The model learned to predict adjustments or offsets to the candidate bounding box coordinates, adjusting their position, size, and aspect ratio.

Subsequently, the refined object proposals were fed into a classification network to assign a label or category to each object. The classfication network extracted meaningful features in the objects and calculated the highest probability of being mapped to one of the predefined classes.

# Trend of Object Detection

<p style="text-align: center;"><img src="/assets/projects/vehicle-detection/Object-Detection-Trend.png" width="800" height="500" /><strong><br />Fig. 1: Trend of deep learning based object detection algorithms <a href="#2">[2]</a>.</strong></p>

Since 2012, various object detection algorithms had been developed as in Fig. 1. Main purposes of them was to improve accuracy, speed, real-time performance, and the ability to detect object at various scales [[2]](#2).


# References
<a name="1"></a>[1] R. Shanmugamani, “Deep Learning for Computer Vision,” *Packt Publishing*, 2018.<br />
<a name="2"></a>[2] R. Sagar, “How the deep learning approach for object detection evolved over the years,” *Analyticsindiamag.com*, https://analyticsindiamag.com/how-the-deep-learning-approach-for-object-detection-evolved-over-the-years/ (accessed Jun. 20, 2023).

