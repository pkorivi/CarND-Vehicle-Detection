
**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Apply color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Normalize the data
* Train and test the model - LinearSVC
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/car_not_car_img.png

[image2]: ./output_images/hog_img1.png
[image9]: ./output_images/hog_img2.png
[image10]: ./output_images/hog_img3.png

[image3]: ./output_images/sliding_1.png
[image4]: ./output_images/sliding_2.png
[image11]: ./output_images/sliding_3.png
[image12]: ./output_images/sliding_4.png

[image5]: ./output_images/heat_1.png
[image13]: ./output_images/heat_2.png
[image14]: ./output_images/heat_3.png
[image15]: ./output_images/heat_4.png
[image16]: ./output_images/heat_5.png


[image6]: ./output_images/labels_map.png

[image7]: ./output_images/all_box1.png
[image17]: ./output_images/all_box2.png
[image18]: ./output_images/all_box3.png
[image19]: ./output_images/all_box4.png
[image20]: ./output_images/all_box5.png


[video1]: ./out_project_video.mp4

---
###Writeup / README

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the 'feature extraction from images' cell of the IPython notebook [car_tracking.ipynb](./car_tracking.ipynb)

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![alt text][image2]
![alt text][image9]
![alt text][image10]

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and evaluated for their performace through test set and run them on the video. Most of the parameters failed for proper detection of cars or detecting too many false positives. I am assume there is no stadard apparoach but worked on varying individual parameter at a time and evaluating performace. As the number of parameters is immense it is impossible to try all the possible combinations. The main things that I tried are changnf color spaces, multiple channel hog features, multiple orientation lists. 

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

The  code for creating data for classifer and training the classifier is in the 'classifier' code cells of IPython notebook [car_tracking.ipynb](./car_tracking.ipynb)

The classifier is trained with combination of hog, spatial color and color histogram features of car and non-car images.

###Sliding Window Search

####1. The implementation for sliding windows to search all the region for the cars in an image is in 'Find cars' cell of the  IPython notebook [car_tracking.ipynb](./car_tracking.ipynb). The function 'find_cars()' takes in image, scale and start and y positions to search a regon for cars. Multiple scale, y_start_stop values are used to cover the image in a sensible and general wayto detect images in any road. 

The below image shows the image with windows for search. 
![alt text][image3]
![alt text][image4]
![alt text][image11]
![alt text][image12]


####2. 
Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result. Here are some examples of test images to demonstrate how pipeline is working. 


![alt text][image7]
![alt text][image17]
![alt text][image18]
![alt text][image19]
![alt text][image20]
---

### Video Implementation

####1. Here's a [link to my video result](./project_video.mp4)


####2. Methods to avoid false detections:

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  The blobs are marked only if they exist in two continious frames. 

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

![alt text][image5]
![alt text][image13]
![alt text][image14]
![alt text][image15]
![alt text][image16]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The main difficulties faced in implementing the pipeline are choosing the right feature set and tuning the parameters for right output. As the system contains multiple steps and parameters tunable at each step, it's leading to hundreds of combinations and most of them turning bad at the end even through providing good results initially or on samples. There are even now some false positives and no detections for certain times, this are failures. The model needs further training and testing.

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

I built the pipleine based on the modules of code prepared during the assignments, tweaking one parameter at a time to check the result and using the combinations working best. I would train the model with more data, choose a model which could do better than this. The model performs way to slow at 1 frame per second which could never be employed in real time scenarios and needs a great amount of optimization for using in the vehicle directly.
