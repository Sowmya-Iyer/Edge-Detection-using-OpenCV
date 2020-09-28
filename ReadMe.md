# Edge Detection
This code snippet is a part of Sign Language Detection project.

ORB is basically a fusion of FAST keypoint detector and BRIEF descriptor with many modifications to enhance the performance. First it use FAST to find keypoints, then apply Harris corner measure to find top N points among them. It also use pyramid to produce multiscale-features. But one problem is that, FAST doesn’t compute the orientation.
It computes the intensity weighted centroid of the patch with located corner at center. The direction of the vector from this corner point to centroid gives the orientation. To improve the rotation invariance, moments are computed with x and y which should be in a circular region of radius r, where r is the size of the patch.


Create an ORB object with the function, cv2.ORB() or using feature2d common interface. It has a number of optional parameters. Most useful ones are nFeatures which denotes maximum number of features to be retained (by default 500), scoreType which denotes whether Harris score or FAST score to rank the features (by default, Harris score) etc. Another parameter, WTA_K decides number of points that produce each element of the oriented BRIEF descriptor. By default it is two, ie selects two points at a time. In that case, for matching, NORM_HAMMING distance is used. If WTA_K is 3 or 4, which takes 3 or 4 points to produce BRIEF descriptor, then matching distance is defined by NORM_HAMMING2.

CODE SNIPPET:
img = cv2.imread('D.png',0)
### Initiate STAR detector
orb = cv2.ORB_create()

### find the keypoints with ORB
kp = orb.detect(img,None)

### compute the descriptors with ORB
kp, des = orb.compute(img, kp)
img2 = cv2.merge([img, img, img])
### draw only keypoints location, not size and orientation
img2 = cv2.drawKeypoints(img,kp,outImage = img2,color=(0,255,0), flags=0)
plt.imshow(img2),plt.axis('off'),plt.show()
import cv2
import numpy as np
### from matplotlib import pyplot as plt

edges = cv2.Canny(img,100,200)

plt.subplot(121),plt.imshow(img,cmap = 'gray')
plt.title('Original Image'), plt.xticks([]), plt.yticks([])
plt.subplot(122),plt.imshow(edges,cmap = 'gray')
plt.title('Edge Image'), plt.xticks([]), plt.yticks([])

plt.show()

