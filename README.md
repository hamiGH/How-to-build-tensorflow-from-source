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

#### Install necessary packages:
   - numpy, which is a numerical processing package that TensorFlow requires.
   - dev, which enables adding extensions to Python.
   - pip, which enables you to install and manage certain Python packages.
   - wheel, which enables you to manage Python compressed packages in the wheel (.whl) format.

To install these packages for Python 2.7, execute the following command:
```sh
     sudo apt-get install python-numpy python-dev python-pip python-wheel
```

To install these packages for Python 3.n, execute the following command:
```sh
     sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel
```

#### Install mock package:
```sh
   pip install -U mock
```

#### Installing CUDA Toolkit by Runfile
Link: [installing cuda toolkit & problem-installing-nvidia-390-42-driver-on-ubuntu-16-04](https://developer.nvidia.com/cuda-zone)

- Download runfile cuda toolkit:
  - Download link: [https://developer.nvidia.com/cuda-zone](https://developer.nvidia.com/cuda-zone)

- Disable the Nouveau drivers: To install the Display Driver, the Nouveau drivers must first be disabled.
  - The Nouveau drivers are loaded if the following command prints anything:
    ```
    $ lsmod | grep nouveau
    ```
  - Create a file at `/etc/modprobe.d/blacklist-nouveau.conf` with the following contents:
    ```
    blacklist nouveau
    options nouveau modeset=0
    ```
  - Regenerate the kernel initramfs:
    ```
    $ sudo update-initramfs -u
    ```
  - Reboot is required to completely unload the Nouveau drivers and prevent the graphical interface from loading.

- Reboot into text mode (runlevel 3):
  - Press `ctrl+alt+f1`
  - Run the following command:
    ```
    $ sudo init 3
    ```

- Stop your graphic service (the X-server):
  - Run the following command:
    ```
    $ sudo service lightdm stop
    ```

- Run the installer: The Runfile installation installs the NVIDIA Driver, CUDA Toolkit, and CUDA Samples. But this removes 390.xx driver and replaces it with 387.xx. So just install the toolkit.
  - Run the following commands:
    ```
    $ chmod +x cuda_<version>_linux.run
    $ sudo sh cuda_<version>_linux.run –toolkit –silent
    ```
  - Go to the next step for installing the CUDA driver.

#### Installing CUDA driver:
- Online installing:
  - Run the following commands:
    ```
    $ sudo add-apt-repository ppa:graphics-drivers/ppa
    $ sudo apt update
    $ sudo apt install nvidia-390
    $ reboot
    ```

#### Installing cuDNN SDK (from a tar file)
Link: [Installing cuDNN on Linux](https://developer.nvidia.com/cudnn)

- Download cuDNN tar file: [https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn)
- Unzip the cuDNN package:
  - Run the following command:
    ```
    $ tar -xzvf cudnn-9.1-linux-x64-v7.1.tgz
    ```
- Copy the following files into the CUDA Toolkit directory:
  - Run the following commands:
    ```
    $ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
    $ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
    ```
  - Rather than using the commands mentioned above, it is recommended to use these commands instead:
     ```
       $ sudo cp cuda/lib64/libcudnn.so.7.1.3 /usr/local/cuda/lib64
       $ sudo cp cuda/lib64/libcudnn_static.a /usr/local/cuda/lib64
       $ sudo ln -s /usr/local/cuda/lib64/libcudnn.so.7.1.3 /usr/local/cuda/lib64/libcudnn.so.7
       $ sudo ln -s /usr/local/cuda/lib64/libcudnn.so.7.1.3 /usr/local/cuda/lib64/libcudnn.so
     ```

- Modify the permissions for the copied files:
  - Run the following command:
    ```
    $ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
    ```

#### Configure CUDA
- Add the CUDA Toolkit to $PATH
    - Open `~/.bashrc` in your favorite editor
        - `$ gedit ~/.bashrc`
    - Add these three export statements to the end of `~/.bashrc`
        ```
            export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
            export LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
            export CUDA_HOME=/usr/local/cuda
        ```
    - Reload the ~/.bashrc file:
        - `$ source ~/.bashrc`

- Configure and edit /etc/environments
    - It is also recommended for Ubuntu users to append string `/usr/local/cuda/bin` to the system file `/etc/environments` so that nvcc will be included in `$PATH`. This will take effect after reboot. To do that, you just have to `sudo gedit /etc/environments` and then add `:/usr/local/cuda/bin` (including the ":") at the end of the `PATH="/blah:/blah/blah"` string (inside the quotes).

#### Test the CUDA Toolkit and cuDNN installation
- Check some version numbers
    - `$ nvcc –version`
    - `$ nvidia-smi`
    - `$ nvidia-smi -l  1`