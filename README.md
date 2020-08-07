# CNN on Jewelry Products README

### Authors
Murtadha


### Date

16th-May-2019

### Website

n/a

### License

GNU General Public License (version 3)-- GPLv3

---


### Problem Statement


Many companies and online stores do their photo-shot in house, cutting cost and accelerate delivery. This usually leads to generic photo-shot setting. Given that the company stores its selling statistics of each  product, can the company use the product photo to maximize selling? This project aims at classifying images of similar setting (e.g. background and brightness), predicting whether this product is going to best selling or not. The practical side of predicting best-selling is that company would take more shots till arriving at a best selling one. The used data is scrapped from Tous online store.

For more information:

- [Tous Jewelry Store - Saudi Arabai](https://www.tous.com/sa-en/)

- [Kaggle pagge of the dataset](https://www.kaggle.com/arimaha/what-makes-a-best-seller-item)

---


### Executive Summary

We first start with importing data into a dataframe and then cleaning some columns and dropping others. We then run EDA to explore the data. We plot two distributions of the data, one of the price and one for the best-selling class.

Secondly, is retrieving images from links. The task of this section is to download images from their links that are in the 'image' column. We will loop over all links and store its corespondent image to two folders: best selling folder and not best selling folder. We we will be using urlib methods for downloading images. First we have to clean the image links from characters that are not part of the url link. Finally, each image is stored with its index as name, and then the path is also stored in the new column 'image_path'.

We notice that the best-selling images have a sticker. Since we are doing CNN on these images, the model will identify this sticker as a pattern for best-selling images. This will results in a model with high variance, which we don't want. The task of the next section is to remove this sticker/tag from all images.

We have to remove the 'best seller' tag from the best-selling products, since it will affect our trainging. We use OpencCV for this task. The main method used is template matching.
We are going to implement a modified Template Matching approach. Here's the overall strategy:
- Load template, convert to grayscale, perform canny edge detection
- Load original image, convert to grayscale
- Continuously rescale image, apply template matching using edges, and keep track of the correlation coefficient (higher value means better match)
- Find coordinates of best fit bounding box then erase unwanted ROI
Much credits of this section go to @nathancy from Stackoverflow.com for his help.

We succeed at removing all tags from the best-selling images. In the next section we continue doing image processing but with Keras image processing libraries. The next step is more focused on how we should feed the images to the model.

We will build a model that classifies images whether they are of best-selling products or not. We first perform some Keras preprocessing on the images, like scaling and validation split. Secondly, we build a simple CNN classifier using the configurations given by the documentation. Thirdly, we carry one empirical experiments, known as sequential experiments, to optimize our model. We finally pick the model of the mos optimized experiment and plot the loss function of all experiments.


Next is CNN classification model. In this section we build the first CNN classification model using the default configurations given by Keras tutorials. Later we optimize our model using sequential experiments methodology. The resulted loss func is 4.7, and the accuracy is 0.39. This mediocre results could be advanced with larger data or empirically trying different hyperparameters. Also, loss function for the validation set keeps increasing linearly.

The aim is to use Sequential experiments approach to optimize CNN classifier model
Tuning hyperparameters of CNN models is an empirical process. Here we carry on multiple experiments to arrive to the optimal hyperparameters. We use the accuracy metric to compare our results.
Summary table of sequential experiments

| Hidden Layers | Nodes Per Layer | Accuracy | Next Step                    |
|---------------|-----------------|----------|------------------------------|
| 1             | 32              | 0.37     | Increase Capacity            |
| 1             | 64              | 0.4      | Increase Capacity            |
| 2             | 64              | 0.4      | Increase Capacity            |
| 3             | 64              | 0.38     | Revoke and increase capacity |
| 3             | 128             | 0.46     | Increase Capacity            |
| 3             | 264             | 0.42     | Revoke and increase capacity |
| 4             | 128             | 0.44     | Done                         |


Finally, CNN image regression (Extra). We will build a CNN regression model to predict prices of each product based on it image. We first do the required Keras preprocessing to feed the model, and then we configure two models. This section has been added for experimental purposes. No optimization has been carried on. Results of the CNN regression: the plot shows poor results of CNN regression model, as the test loss fun deviates from the training loss fun. However, an early stop could be implemented since the loss fun doesn't get better at some points. With some empirical trials on the CNN hyperparametrs, we might arrive at better results. We leave that for future tuning!

---

### Data Source


This data was scrapped and published on Kaggle on 2019-04-18. The last version was updated on 2019-05-03, of which this code is based. [Link to the dataset Kaggle page.](https://www.kaggle.com/arimaha/what-makes-a-best-seller-item)

---

### Data Dictionary



| Feature     | Type    | Dataset | Description                                |
|-------------|---------|---------|--------------------------------------------|
| best_seller | object | data    | 1 if this item is best-seller, otherwise 0 |
| price       |  float64 | data    | price of the item                          |
| image       |  object | data    | link to the image of the item              |
| image_path    | object | data    | path of the downled image at local storage       |


---

### Observations/Conclusions

After running 7 sequential experiments to optimize CNN classification model, we decided to chose model of the 5th experiment to be the optimal. We succeeded in achieving 42% accuracy.

---

### Acknowledgment

This project was submitted as part of the requirements of the General Assembly Data Science immersive course. For 3 months I have been surrounded by energetic inspiring colleagues and instructors. I have learned a lot, and most importantly, I have gained life-time friendship. I would like to thank all General Assembly staff, specially (in alphabetical order): Andrew, Esraa, Khaled, and Spencer; without you we would be still just a simple linear model! Thanks a lot.
