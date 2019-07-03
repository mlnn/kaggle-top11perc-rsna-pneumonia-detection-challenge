## OVERVIEW

TOP 11% (1% to bronze medal) in competition RSNA Pneumonia Detection Challenge

# Description

In this competition, you’re challenged to build an algorithm to detect a visual signal for pneumonia in medical images. Specifically, your algorithm needs to automatically locate lung opacities on chest radiographs.

Here’s the backstory and why solving this problem matters.

Pneumonia accounts for over 15% of all deaths of children under 5 years old internationally. In 2015, 920,000 children under the age of 5 died from the disease. In the United States, pneumonia accounts for over 500,000 visits to emergency departments [1] and over 50,000 deaths in 2015 [2], keeping the ailment on the list of top 10 causes of death in the country.

While common, accurately diagnosing pneumonia is a tall order. It requires review of a chest radiograph (CXR) by highly trained specialists and confirmation through clinical history, vital signs and laboratory exams. Pneumonia usually manifests as an area or areas of increased opacity [3] on CXR. However, the diagnosis of pneumonia on CXR is complicated because of a number of other conditions in the lungs such as fluid overload (pulmonary edema), bleeding, volume loss (atelectasis or collapse), lung cancer, or post-radiation or surgical changes. Outside of the lungs, fluid in the pleural space (pleural effusion) also appears as increased opacity on CXR. When available, comparison of CXRs of the patient taken at different time points and correlation with clinical symptoms and history are helpful in making the diagnosis.

CXRs are the most commonly performed diagnostic imaging study. A number of factors such as positioning of the patient and depth of inspiration can alter the appearance of the CXR [4], complicating interpretation further. In addition, clinicians are faced with reading high volumes of images every shift.

To improve the efficiency and reach of diagnostic services, the Radiological Society of North America (RSNA®) has reached out to Kaggle’s machine learning community and collaborated with the US National Institutes of Health, The Society of Thoracic Radiology, and MD.ai to develop a rich dataset for this challenge.

The RSNA is an international society of radiologists, medical physicists and other medical professionals with more than 54,000 members from 146 countries across the globe. They see the potential for ML to automate initial detection (imaging screening) of potential pneumonia cases in order to prioritize and expedite their review.

# Evaluation

This competition is evaluated on the mean average precision at different intersection over union (IoU) thresholds. The IoU of a set of predicted bounding boxes and ground truth bounding boxes is calculated as: IoU(A,B)=A∩BA∪B.

The metric sweeps over a range of IoU thresholds, at each point calculating an average precision value. The threshold values range from 0.4 to 0.75 with a step size of 0.05: (0.4, 0.45, 0.5, 0.55, 0.6, 0.65, 0.7, 0.75). In other words, at a threshold of 0.5, a predicted object is considered a "hit" if its intersection over union with a ground truth object is greater than 0.5.

At each threshold value, a precision value is calculated based on the number of true positives (TP), false negatives (FN), and false positives (FP) resulting from comparing the predicted object to all ground truth objects.

A true positive is counted when a single predicted object matches a ground truth object with an IoU above the threshold. A false positive indicates a predicted object had no associated ground truth object. A false negative indicates a ground truth object had no associated predicted object.

Important note: if there are no ground truth objects at all for a given image, ANY number of predictions (false positives) will result in the image receiving a score of zero, and being included in the mean average precision.

The average precision of a single image is calculated as the mean of the above precision values at each IoU threshold.

In your submission, you are also asked to provide a confidence level for each bounding box. Bounding boxes will be evaluated in order of their confidence levels in the above process. This means that bounding boxes with higher confidence will be checked first for matches against solutions, which determines what boxes are considered true and false positives.

NOTE: In nearly all cases confidence will have no impact on scoring. It exists primarily to allow for submission boxes to be evaluated in a particular order to resolve extreme edge cases. None of these edge cases are known to exist in the data set. If you do not wish to use or calculate confidence you can use a placeholder value - like 1.0 - to indicate that no particular order applies to the evaluation of your submission boxes.

Lastly, the score returned by the competition metric is the mean taken over the individual average precisions of each image in the test dataset.

Intersection over Union (IoU)

Intersection over Union is a measure of the magnitude of overlap between two bounding boxes (or, in the more general case, two objects). It calculates the size of the overlap between two objects, divided by the total area of the two objects combined.


The two boxes in the visualization overlap, but the area of the overlap is insubstantial compared with the area taken up by both objects together. IoU would be low - and would likely not count as a "hit" at higher IoU thresholds.

# Data

30.2k images and 30.2k x 6 description for train
3000 images for test

## Solution

- pydicom to convert images
- Mask_RCNN with mask_rcnn_coco.h5 to construct bounding boxes
- BACKBONE resnet50
- playing with hyperparameters: IMAGE_MIN_DIM, IMAGE_MAX_DIM, RPN_ANCHOR_SCALES, TRAIN_ROIS_PER_IMAGE, MAX_GT_INSTANCES, DETECTION_MAX_INSTANCES, DETECTION_MIN_CONFIDENCE, DETECTION_NMS_THRESHOLD
- trying different epochs: from 10 to 20 (the best is 20) 

## Result

Private - 0.15175
Public - 0.11284

# 159 of 346 and TOP11%
