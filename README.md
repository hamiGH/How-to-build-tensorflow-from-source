# How to build tensorflow from source

## Setup information
- OS Platform and Distribution (e.g., Linux Ubuntu 16.04): Ubuntu 16.04
- TensorFlow version: r1.8
- Python version: 3.6.4
- Bazel version: 0.15.2
- GPU model and memory: Titan V
- CUDA/cuDNN version: 9.1 / 7.1.3
- GCC/Compiler version: 4.9.4
- NVIDIA Driver Version: recommended 390.77
- NCCL version: 2.1.15

## Install Bazel
Find bazel version according to tensorflow version → [link for bazel version](link)

Link → Using Bazel custom APT repository

- Step 1: Install the JDK(8)
```sh
sudo apt-get install openjdk-8-jdk
```

- Step 2: Add Bazel distribution URI as a package source
```sh
echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
```

- Step 3: Install and update Bazel
```sh
sudo apt-get update
sudo apt-get install bazel
```
once installed, you can upgrade to a newer version of Bazel with the following command:
```sh
sudo apt-get upgrade bazel
```

## Install TensorFlow dependencies
### 1. To install TensorFlow, you must install the following packages:
   - numpy, which is a numerical processing package that TensorFlow requires.
   - dev, which enables adding extensions to Python.
   - pip, which enables you to install and manage certain Python packages.
   - wheel, which enables you to manage Python compressed packages in the wheel (.whl) format.

To install these packages for Python 2.7, issue the following command:
```sh
     sudo apt-get install python-numpy python-dev python-pip python-wheel
```

To install these packages for Python 3.n, issue the following command:
```sh
     sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel
```

### 2. Installing mock package:
```sh
   pip install -U mock
```
