# Hack your Look: An Image-based Jewelry Recommender

by Linh Chau (https://www.linkedin.com/in/linhchau/)


Jewelry is an important part of an outfit. However, compared to other wardrobe pieces, jewelry can be prohibitably expensive and difficult to shop for. I'm building a recommendation system based off an image classifier to help shoppers find jewelry pieces, specifically earrings and necklaces, that are similar in image to pieces they can input.

First, I created an image classifier with a convolutional neural network to identify what type of jewelry the input image is. Then I use an autoencoder convolutional neural network to reduce the input image to a specific set of features. The reduced features are used as input into a knn to identify jewelry in the training set that are similar visually and are in a specific price range. 

![Image Recommender Pipeline](https://github.com/pugzillo/jewelery_recommender/blob/master/images/Jewelry_pipeline.jpg "Image Recommender Pipeline")


## Data
Scraped images and metadata for earrings (N = 9401) and necklaces (N = 6937) from [ShopStyle](https://www.shopstyle.com/ "Shop Style"). 90% of earrings and necklaces were used for the training set (N = 23,155) and 10% of each type were used for the testing set. 

## Building a Jewelry Classifier with a CNN Classifier
A convolutional neural network (CNN) was used to identify if an image contained a necklace or earring. For training of the CNN, segmentation with cv2 library was performed on earring images given that they come in pairs. The background for earring images was masked out and then I identified the contours of the images, which were converted to bounding boxes. The top two bounding boxes were then selected as input for the earring training set. 

All images were converted to RGB and resized to 100x100.

The model was ran for 15 epochs.

### CNN Classifier Architecture
![CNN Classifer architecture](https://github.com/pugzillo/jewelery_recommender/blob/master/images/jewelry_cnn_classifier_final.png "Architecture CNN Classifier")

### CNN Classifier Evaluation
![alt text](https://github.com/pugzillo/jewelery_recommender/blob/master/images/CNN_classifier_model_loss_graph.png "Log Loss for CNN Classifier")

There is some oscillation in the log loss graph, which may suggest that there is high confidence in misclassified images. Therefore, this is a place for improvement in this classifer model.

## Image Feature Reduction with CNN Autoencoder
A convolutional neural network autoencoder was used to reduce the dimensions of each training and input image. 

### CNN Autoencoder Architecture
![CNN Autoencoder](https://github.com/pugzillo/jewelery_recommender/blob/master/images/cnn_autencoder_model_final.png "Architecture CNN Autoencoder")

### CNN Autoencoder Evaluation
![alt text](https://github.com/pugzillo/jewelery_recommender/blob/master/images/CNN_autoencoder_model_loss.png "Log Loss for CNN Autoencoder")


## Building a Recommender with KNN

K-nearst neighbors was used to identify images, with reduced features from the cnn autoencoder, that are similar to the input image. The **cosine similarity** was used as the distance measure. Once the closest neighbors were identified, a price filter was applied. 

## Running pipeline for a novel image
`python3 jewelry_recommender_novel_image.py`

## Conclusions and Future Work
Even though I had a small set of training images, I was about to build a recommender using a KNN algorithm with features created by the dimension reduction capabilites of a CNN autoencoder. This allows users to input pictures of jewelry that they are interested in and get similar pieces in a certain price range with links for purchase. 

In the future, I'd like to focus on the following:
1. The CNN can be further tuned with more training images or with other types of jewelry (ie. rings).
2. Using transfer learning can potentially used to detect type of jewelry.
3. Adding features concerning non visual aspects of the jewelry pieces can be added (ie. brand).


