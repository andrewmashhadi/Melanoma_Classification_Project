# SIIM-ISIC Melanoma Classification Project
## Identifying melanoma from lesion images


The official ISIC 2020 Challenge Dataset for Skin Lesion Analysis Towards Melanoma Detection: *https://challenge2020.isic-archive.com/*

The goal is to identify melanoma in images of skin lesions. In particular, we want to use images within the same patient and determine which are likely to represent a melanoma. Using patient-level contextual information may help the development of image analysis tools, which could better support clinical dermatologists.

## Modeling

Melanoma is a deadly disease, but if caught early, most melanomas can be cured with minor surgery. Image analysis tools that automate the diagnosis of melanoma will improve dermatologists' diagnostic accuracy. Better detection of melanoma has the opportunity to positively impact millions of people.

Our model consists of a Two-Network Ensemble. The first network trains a convolutional neural network using only the image data, and the second network trains a single layer multi-layer perceptron (the number of layers and neurons for this MLP network was tuned). Then, the two networks are joined for one last layer before generating predictions (number of neurons also tuned). This general architecture has been used in the past. However, in this case, I have decided to use the all-new 2022 ResNeSt network for the CNN. Additionally, I trained a similar model with a basic CNN structure to compare the results to the ResNeSt results. 

Therefore, the two CNN models trained were:

### Convolutional Neural Network with Image Augmentation, Batch Normalization, Adam Optimizer and L2 Regularization

This network uses 6 convolutional filters (kernel sizes of 7 and 5), and 3 linear layers. I use ReLU activation and Max-Pooling (the final layer uses sigmoid for probabilities). Binary Cross Entropy loss is used with an Adam optimizer using weight decay (for the L2 regularization) and a scheduler to lower the learning rate over time. Randomized over-sampling was used to help alleviate the class imbalance. More involved image augmentation and batch normalization were also used in this network.

Current results: I used the validation set results to tune many hyper-parameters. From these results, I decided to fix the training to 11 epochs when training on the entire training set. I then trained our network on the entire training set and acheived a *0.87* AUC score on the test set. See the associated notebook for more details and results.

### ResNeSt269: Split Attention Network

I used the ResNeSt-269 model described here: https://arxiv.org/abs/2004.08955. My model fine-tuned pre-trained ResNeSt-269 parameters from the IMAGENET dataset. This model only allows for a crop size of 416x416. Therefore, during training we randomly cropped our 512x512 input sizes to 416x416 resolution images. The Binary Cross Entropy loss was used with the Adam optimizer as well, however, to not heavily modify the pre-trained weights from the ResNeSt model, I assigned a very small learning rate to the pretrained weights and a larger learning rate for all other weights.

Current results: I used the validation set results to tune many hyper-parameters. From these results, I decided to fix the training to 12 epochs when training on the entire training set. I then trained our network on the entire training set and acheived a *0.93* AUC score on the test set. See the associated notebook for more details and results.

## A Note About Computing and Data Storage

Due to UCLA (and my) limited resources, I used a mixture of the UCLA's Hoffman2 Linux Cluster and Google Colab Pro+ to tune, train, and test the above networks with affordable GPUs and affordable storage. The resolution of the image data varied between 128x128 to 4000x6000, and there is about 33000 images. Ultimately, I found that the only way to train efficiently with suffcient batch-size (16+ images per batch) was to center cropped or resize (interpolation from cv2.resize()) the original lesion images to 512x512. This saved training time, and allowed for optimal results from my models. The GPU changed occasionally, but most of the time I used a single NVIDIA A100 GPU with about 40GB GPU RAM.

The training/testing split was set to 80/20 percent. While validating, the validation set consisted of an additional split from the training set. Once hyper-parameters were tuned, the entire training set was used to train the networks and ultimately make predictions on the test set.
