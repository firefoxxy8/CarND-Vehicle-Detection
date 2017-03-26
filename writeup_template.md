##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/car_not_car.png
[autti]: ./examples/autti-dataset.png
[hog]: ./examples/hog.png
[sliding_window]: ./examples/sliding_window.png
[pipeline]: ./examples/pipeline.png
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.
I used the default dataset and the [autti](https://github.com/udacity/self-driving-car/tree/master/annotations) one. The autti dataset
it's more completed then the default one.
![][autti]
I started by reading in all the `car` and `non-car` images.  Here is an example of one of each of the `car` and `non-car` classes:

![alt text][image1]





####2. Explain how you settled on your final choice of HOG parameters.

My feature vector consist of 128 components which I extract from grayscaled images.

The parameters are contained in the code cell 20 of the Jupyter notebook `vehicle_detection.ipynb`.  

I used grayscaled images beacause cars have many colors, so it
cannot be considered as an important feature.

![alt text][hog]

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a SVM classifier with rbf kernel and default sklearn parameters.
I have combined default and AUTTI datasets with the result of 13790 cars and 48163 non-cars images for training.  
Using the StandardScaler for feature normalization along resulting dataset adds about 4% accuracy for my classifier.
Resulting accuracy is about 98%.

The code is contained in the code cells 22-23 of the Jupyter notebook `vehicle_detection.ipynb`.  

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

To look for cars in the images I implemented a sliding window
search.
I used a 3 square sliding window sizes of 128, 96 and 80 pixels side size. While iterating I use 50% window overlapping in horizontal and vertical directions. The area where sliding window search is performed is where the probability to find a car is big.

![alt text][sliding_window]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

These are the images of the images pipeline
![alt text][pipeline]

After the sliding window search, we have hot boxes classified
containing a car but with some false positives.
We need to join overlapped boxes estimating a box size. Cell 27
Also augmenting the non car datas in dataset help to lower the false positives
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_result.mp4)


---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I think this method is a good start but it is slow and cannot be used for realtime processing.

I've read the YOLO method and planning to study it and apply it in the future beacuse it's seem very stable and fast.
