# tensorflow-installation-from-source-compute3.0

Tensorflow is a popular deep learning framework that can be generally be installed via pip. However, systems with old GPUs of compute capabilities 3.0 need to build their installation of Tensorflow from source. This guide outlines my process and common troubleshooting advice.

System descripton:
python: 2.7.17
python3: 3.6.9
cuda: 10.2
tensorflow: 2.1.0
bazel: 3.0.0
GPU: Nvidia GeForce GTX 660

Much of this follows https://www.tensorflow.org/install/source, 

First, update python.
sudo apt install python-dev python-pip
