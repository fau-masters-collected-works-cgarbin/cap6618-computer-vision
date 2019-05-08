# CAP6618 Machine Learning for Computer Vision Spring 2019

Most of this work is based on our textbook, [_Hands-on machine learning wiht scikit-learn, Keras, and TensorFlow_](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/) (author's [GitHub repo](https://github.com/ageron/handson-ml)). Highly recommended.

## Module 3

MNIST digit classification without using neural networks.

Using only traditional classifiers from scikit-learn (any classifiter, except for `MLPClassifier`).

## Module 4

MNIST digit classification using denseley-connected neural networks, but not deep learning (e.g. CNNs).

Using scikit-learn `MLPClassifier` for classification and `GridSearchCV` for hyperparameter optimization.

## Module 5

MNIST digit classification using CNNs.

Comparing densely-connected (`Dense` layers only) with CNNs (`Conv2D` and `MaxPooling2D`), regularization with `Dropout` and `BatchNormalization` and trainig with `EarlyStopping`.

CNNs tested are based on [LeNet-5](http://yann.lecun.com/exdb/lenet/) and [VGG-13](https://arxiv.org/abs/1409.1556).

## Module 6

Transfer learning and feature extraction with CNNs, including pre-trained CNN models.

Two flavors of transfer learning:

- Feature extraction: remove the dense layers of Keras' [VGG-16](https://keras.io/applications/#vgg16) pretrained on [ImageNet](http://www.image-net.org/), leaving only the convolutional layers. Use these layers for feature extraction and train a dense layer to predict based on the extracted features.
- Partial retraining: unfreeze the last convolutional layer and retrain it, together with a dense layer for the final prediction.

## Module 7

More transfer learning, this time experimenting with different datasets on a pretrained Inception V3 network.

Switch to a more powerful network, based on [Inception V3](https://arxiv.org/abs/1512.00567), and experiment with transfer learning using images from different domains.

## Module 8 TensorFlow.js

Final project, "Machine learning in a constrained environment", where "constrained environment" is an environment in which we don't have access to the full resources of a computing environment (full access to the hardware in a laptop, desktop or server).

For this assignment, we picked the web browser as the constrained environment and TensorFlow.js as the means to work in this environment. The project documents the how to train, test, debug, and release models in production in this environment.
