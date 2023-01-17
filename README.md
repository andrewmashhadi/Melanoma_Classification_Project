# SIIM-ISIC Melanoma Classification Project
## Identifying melanoma from lesion images


The official ISIC 2020 Challenge Dataset for Skin Lesion Analysis Towards Melanoma Detection: *https://challenge2020.isic-archive.com/*

The goal is to identify melanoma in images of skin lesions. In particular, we want to use images within the same patient and determine which are likely to represent a melanoma. Using patient-level contextual information may help the development of image analysis tools, which could better support clinical dermatologists.

Melanoma is a deadly disease, but if caught early, most melanomas can be cured with minor surgery. Image analysis tools that automate the diagnosis of melanoma will improve dermatologists' diagnostic accuracy. Better detection of melanoma has the opportunity to positively impact millions of people.


This project will try a few different models:

### Basic Convolutional Neural Network with Stochastic Gradient Descent

This network uses 9 convolutional filters (kernel size of 5), and 3 linear layers. I use ReLU activation and Max-Pooling (the final layer uses sigmoid for probabilities). Binary Cross Entropy loss is used with regular stochastic gradient descent with a scheduler to lower the learning rate over time. Randomized over-sampling was used to help alleviate the class imbalance.

Current results: After 5 epochs of training, the validation loss plateaued. We tested our network from 2.5 epochs of training, and achieved a *0.837* AUC score. See the associated Jupyter notebook for more results.

### Convolutional Neural Network with Image Augmentation, Batch Normalization, Adam Optimizer and L2 Regularization

This network uses 9 convolutional filters (kernel size of 5), and 3 linear layers. I use ReLU activation and Max-Pooling (the final layer uses sigmoid for probabilities). Binary Cross Entropy loss is used with an Adam optimizer using weight decay (for the L2 regularization) and a scheduler to lower the learning rate over time. Randomized over-sampling was used to help alleviate the class imbalance. More involved image augmentation and batch normalization were also used in this network.

Current results: We stopped training after 10 epochs. However, the validation loss trend did not completely plateau (may train further later on). We tested our network from the 10th epoch of training, and achieved a *0.87* AUC score. See the associated Jupyter notebook for more results.

### Residual Network 50

This ResNet50 model is from the "Deep Residual Learning for Image Recognition" paper here: https://arxiv.org/pdf/1512.03385.pdf. Feature extraction was tested, and did not provide very strong results. Fine-tuning, however, using 3 additional linear layers (with batch-normalization and dropout) demonstrated great results. Given that the ResNet50 was trained with 224x224 resolution images, we center cropped and resized (interpolation from cv2.resize()) the original lesion images for training. This saved training time, and allowed for optimal results from the ResNet50 model. Training and hyper-parameter tuning was done.

Current results: After 15 epochs, the validation loss trend haf plateaued. The ROC AUC score is at 0.89. See the associated Jupyter notebook for more results.

### Residual Network 152

Will use fixed hyper parameters and additional layers from the ResNet50 fine-tuning model to train a ResNet152 model (for fine-tuning and feature extraction). Unfortunately, the optimal input for this model is still 224x224, so we will again use the interpolated data.

Current results: After 10 epochs, the validation loss trend had plateaued. The ROC AUC score remained at about 0.88. See the associated Jupyter notebook for more results.

### Efficient Network V2 (2022)

Will *try* the Version 2 EfficientNet from https://arxiv.org/abs/2104.00298 for feature extraction and fine-tuning. We can use the V2Small model with 384x384 sized images and the V2Medium model with 480x480 sized images.

### ResNeSt (2022)

Will try the ResNeSt-269 model from https://arxiv.org/abs/2004.08955 and https://pytorch.org/hub/pytorch_vision_resnest/ for feature extraction and fine-tuning. This would allow for a crop size of 416x416.

Current results: After 1 epochs, the validation loss trend has not plateaued yet. Training and hyper-parameter tuning is ongoing. See the associated Jupyter notebook for more results.
