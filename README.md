# SIIM-ISIC Melanoma Classification Project
## Identifying melanoma from lesion images


Based on Kaggle competition here: *https://www.kaggle.com/competitions/siim-isic-melanoma-classification/overview/description*

The goal is to identify melanoma in images of skin lesions. In particular, we want to use images within the same patient and determine which are likely to represent a melanoma. Using patient-level contextual information may help the development of image analysis tools, which could better support clinical dermatologists.

Melanoma is a deadly disease, but if caught early, most melanomas can be cured with minor surgery. Image analysis tools that automate the diagnosis of melanoma will improve dermatologists' diagnostic accuracy. Better detection of melanoma has the opportunity to positively impact millions of people.

This project will try four different models:

### Basic Convolutional Neural Network with Stochastic Gradient Descent

This network uses 9 convolutional filters (kernel size of 5), and 3 linear layers. I use ReLU activation and Max-Pooling (the final layer uses sigmoid for probabilities). Binary Cross Entropy loss is used with regular stochastic gradient descent with a scheduler to lower the learning rate over time. Randomized over-sampling was used to help alleviate the class imbalance.

Current results: After 5 epochs of training, the validation loss plateaued. We tested our network from 2.5 epochs of training, and achieved a *0.837* AUC score. See the associated Jupyter notebook for more results.

### Convolutional Neural Network with Image Augmentation, Batch Normalization, Adam Optimizer and L2 Regularization

This network uses 9 convolutional filters (kernel size of 5), and 3 linear layers. I use ReLU activation and Max-Pooling (the final layer uses sigmoid for probabilities). Binary Cross Entropy loss is used with an Adam optimizer using weight decay (for the L2 regularization) and a scheduler to lower the learning rate over time. Randomized over-sampling was used to help alleviate the class imbalance. More involved image augmentation and batch normalization were also used in this network.

Current results: Training as of 1/2/2023.

### Residual Network

Have not decided on whether I will build and train my own ResNet manually, fine-tune pre-trained weights from the ImageNet ResNet50 network, or use the ImageNet ResNet50 network weights and only update the final layer weights from which we derive predictions. Depends on available time.

### Efficient Network

Will only be using the EffNet B3-B7 network's pre-trained weights and updating the final layer weights. Depends on available time.
