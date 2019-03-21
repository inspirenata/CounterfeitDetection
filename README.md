# NewRepository

Counterfeit products is a big problem for many industries. Our company, Heroes BV was asked to bullt a solution to identify fake products for a global Electronics brand by just taking one picture on a smartphone. This repository shows the approach we used to achive it.

Let say we only have a front photo of a product's package:


Counterfeit packages might have mistakes in text and some text might be missed as well. To check this we extract printed text with optical character recognition (OCR) from an image by using Computer Vision's REST API by Azure Cognitive Services.
With Computer Vision's REST API we not only get the extracted text but also the coordinates of the bounding boxes around this text. We save these coordinates to use it further.


It is visible that our package is a bit rotated and is not exactly upfront. That can cause problems for our further processing. To stabilize the image we need to apply geometrical transformations such as affine transformations, perspective transformations and undistortion. To implement this we use bounding box coordinates of Logo and other text,  Different combinations are possible depending on location and amount of text on the front side of the package. 

```
pts1 = np.float32([[50,50],[200,50],[50,200]])
pts2 = np.float32([[10,100],[200,50],[100,250]])

M = cv2.getAffineTransform(pts1,pts2)

dst = cv2.warpAffine(img,M,(cols,rows))
```
The result:

Now we start with finding position of different elements on the package. For that we calculete the bouning box (BB) coordinates around the whole box based on proportions between size of text elements and whole box of original package. To find the coordinates of BB around the whole box it is also possible to use 



## Built With

* [Azure Computer Vision API](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts/python-print-text) - The web framework used
* [OpenCV](https://opencv.org/) - Dependency Management



