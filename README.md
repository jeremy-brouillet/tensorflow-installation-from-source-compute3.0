# tensorflow-installation-from-source-compute3.0

Tensorflow is a popular deep learning framework that can be generally be installed via pip. However, systems with old GPUs of compute capabilities 3.0 need to build their installation of Tensorflow from source. This guide outlines my process and common troubleshooting advice.

System descripton:  
Ubuntu: 18.04.4 LTS
python: 2.7.17  
python3: 3.6.9  
cuda: 10.2  
tensorflow: 2.1.0  
bazel: 3.0.0  
GPU: Nvidia GeForce GTX 660

Much of this follows https://www.tensorflow.org/install/source, 

First, update python.
```sh
sudo apt install python3-dev python3-pip
```

Install additional packages.
```sh
pip install -U --user pip six numpy wheel setuptools mock 'future>=0.17.1'
pip install -U --user keras_applications --no-deps
pip install -U --user keras_preprocessing --no-deps
```

Bazel is used to automatically build tensorflow. Download Bazel 3.0.0 from https://github.com/bazelbuild/bazel/releases/tag/3.0.0. Tensorflow 2.1.0 is only compatible with Bazel 3.0.0, so it is necessary to get the specific release we need. 
```sh
chmod +x bazel-3.0.0-installer-linux-x86_64.sh
./bazel-3.0.0-installer-linux-x86_64.sh --user
```
As Bazel is installed in HOME/bin, we need to add Bazel to the path.
```sh
export PATH="$PATH:$HOME/bin"
```

Download Tensorflow.
```sh
git clone https://github.com/tensorflow/tensorflow.git
```
Install CUDA
Follow the instuctions on the nvidia page to find the proper version of CUDA to install.
https://developer.nvidia.com/cuda-downloads

Install GPU driver
Find your Nvida driver:
https://www.nvidia.com/en-us/drivers/results/159360/

Configure.
```sh
./configure
```
The default python selected is incorrect, as python will default to python2.7. You need to enter /usr/bin/python3.6. Hit enter to stay with the default options until it asks:
```sh
Do you wish to build TensorFlow with CUDA support? [y/N]:
```
Enter y to install CUDA.

At this point, you will have an updated version of Python, Bazel installed, Tensorflow downloaded, CUDA installed, the GPU driver installed, and a configured Bazel file. At this point, to begin the build, enter

```sh
bazel build //tensorflow/tools/pip_package:build_pip_package --verbose_failures --config=opt
```
This triggers the build process, which can take several hours depending on your system. The build may fail. In which case, it's recommended you run:
```sh
bazel clean
```
The most easily addressible build failure is a missing package. Use pip3 to install the missing package and retry the build. 

Once the build has completed, run the wheel you have created:
```sh
pip install /tmp/tensorflow_pkg/tensorflow-version-tags.whl
```
This will install Tensorflow with GPU support onto your system. Try:
```sh
python3 -c "import tensorflow as tf;print(len(tf.config.experimental.list_physical_devices('GPU')))"
```
If your GPU is working, this should print a 1, indicating you have one GPU running.
