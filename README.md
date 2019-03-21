# NewRepository

##Getting started
Counterfeit products is a big problem for many industries. Our company, Heroes BV was asked to bullt a solution to identify fake products for a global Electronics brand by just taking one picture on a smartphone. This repository shows the approach we used to achive it.

## Text  recognition
Let say we only have a front photo of a product's package:
![alt text](https://camo.githubusercontent.com/d4f8564ac8814286de6fe9868ed51c653923eb89/687474703a2f2f74686568756e74696e67746f6e69616e2e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f31322f7061636b6167655f7069632e6a7067)

Counterfeit packages might have mistakes in text and some text might be missed as well. To check this we extract printed text with optical character recognition (OCR) from an image by using Computer Vision's REST API by Azure Cognitive Services. and compare the extracted text with the text stored in our database.


With Computer Vision's REST API we not only get the extracted text but also the coordinates of the bounding boxes around this text. We save these coordinates to use it further.


## Image preprocessing
It is visible that our package is a bit rotated and is not exactly upfront. That can cause problems for our further processing. To stabilize the image we need to apply geometrical transformations such as affine transformations, perspective transformations and undistortion. To implement this we use bounding box coordinates of Logo and other text,  Different combinations are possible depending on location and amount of text on the front side of the package. 

```
pts1 = np.float32([[50,50],[200,50],[50,200]])
pts2 = np.float32([[10,100],[200,50],[100,250]])

M = cv2.getAffineTransform(pts1,pts2)

dst = cv2.warpAffine(img,M,(cols,rows))
```
The result:


## Creating Deep learning model with Tensorflow
Our package has interesting elements such as logo, icons and technical image. We need to check if these elements are genuine or not. To implement this check we crop the part of the image with particular element using BB coordinates and feed the AI model.

The most crucial moment is to create a good dataset to train and test the model. For the classification problem that we have we need to collect images for at least 2 classes: fake and genuine. A presence of a good examples of images for both classes is very important to achieve the high accuracy of the model. 

It is hard to collect/create a big amount of images for each class, that is why we apply Transfer learning approach to train the model. Within this project we developed custom models using Tensorflow, but it is also possible to train classification model using [Azure Custom Vision Service.](https://www.customvision.ai/)



## Find position of the elements
Now we start with finding position of different elements on the package. For that we calculate the bouning box (BB) coordinates around the whole box based on proportions between size of text elements and whole box of original package. To find the coordinates of BB around the whole box it is also possible to use Object Detection feature from Computer Vision API.






## Built With

* [Azure Computer Vision API](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts/python-print-text) - The web framework used
* [OpenCV](https://opencv.org/) - Dependency Management



