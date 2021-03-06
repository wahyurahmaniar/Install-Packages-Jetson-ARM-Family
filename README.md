# Install-Packages-Jetson-ARM-Family
The objective is to give you clear instruction on how to install packages in ARM platform, especially in Jetson family. This instruction was done under Python 3. Tests have been made on Jetson TX2 and Jetson Xavier. You may change ```sudo python3``` and ```sudo pip3``` into ```sudo python2``` and ```sudo pip```, respectively, to make it work under Python 2.

## Dependencies Installation
Before performing any installations, you may need to install the basic dependencies first.
```
$ sudo apt-get install cmake
$ sudo apt-get install python3-pip
$ sudo pip3 install wget
$ sudo pip3 install Cython
```

## PyCUDA Installation
```
$ sudo apt-get install libboost-all-dev
$ sudo apt-get install python-numpy
$ sudo apt-get install build-essential python-dev python-setuptools libboost-python-dev libboost-thread-dev
```

In the same directory of your PyCUDA download, run this terminal
```
# JetPack 4.4
$ export CUDA_HOME=/usr/local/cuda-10.2 
$ export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64:$LD_LIBRARY_PATH
$ export PATH=/usr/local/cuda-10.2/bin:$PATH
$ wget https://files.pythonhosted.org/packages/c8/35/130ac8867b30f9c6ae699b689633a803a73787533f41e52afcf28b75bd81/pycuda-2019.1.1.tar.gz
$ tar xzvf pycuda-2019.1.1.tar.gz
$ cd pycuda-2019.1.1
```
Open configure.py and change the /usr/bin/env python into /usr/bin/env python3
```
$ ./configure.py
$ make -j4
$ sudo python3 setup.py install
$ sudo pip3 install .
```

## LLVM Installation
In this tutorial, I used LLVM 7.0.1.
```
$ wget http://releases.llvm.org/7.0.1/llvm-7.0.1.src.tar.xz
$ tar -xvf llvm-7.0.1.src.tar.xz
$ cd llvm-7.0.1.src
$ mkdir llvm_build_dir
$ cd llvm_build_dir/
$ cmake ../ -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="ARM;X86;AArch64"
$ make -j4
$ sudo make install
$ cd bin/
$ echo "export LLVM_CONFIG=\""`pwd`"/llvm-config\"" >> ~/.bashrc
$ echo "alias llvm='"`pwd`"/llvm-lit'" >> ~/.bashrc
$ source ~/.bashrc
$ sudo pip3 install llvmlite==0.30.0
```

## Numba Installation
Before proceeding to the Numba installation, you need to perform LLVM installation above first since Numba is heavily rely on LLVM installation.
```
$ sudo pip3 install numba==0.48
```

## Protobuf Installation
You may install Protobuf directly from Python using pip:
```
$ sudo pip3 install protobuf
```
If that doesn't work, you may build from source:
```
$ git clone https://github.com/protocolbuffers/protobuf.git
$ cd protobuf
$ git submodule update --init --recursive
$ ./autogen.sh
$ ./configure
$ make -j4
$ make check
$ sudo make install
$ sudo ldconfig
```

## ONNX Installation
Before proceeding to the ONNX installation, you need to perform Protobuf installation above first since ONNX heavily relies on Protobuf installation.
```
$ sudo apt-get install libprotoc-dev protobuf-compiler
$ sudo pip3 install pybind11
$ sudo pip3 install onnx==1.4.1
```

## Keras Installation
```
$ sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev
$ sudo apt-get install libblas-dev liblapack-dev libatlas-base-dev gfortran
$ sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev
$ sudo pip3 install scipy==1.5
$ sudo pip3 install keras
```

## Tensorflow Installation
```
$ sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev
$ sudo pip3 install -U numpy==1.16.1 future==0.17.1 mock==3.0.5 h5py==2.9.0 keras_preprocessing==1.0.5 keras_applications==1.0.6 enum34 futures testresources setuptools protobuf
```
If you have trouble installing Protobuf, you may take a look at the Protobuf installation part above. If everything's fine, you may proceed. Now, take a look at this https://developer.download.nvidia.com/compute/redist/jp/v44/tensorflow/. You may find a lot of JetPack versions and you need to choose one based on your preference. 

```
# JetPack 4.4
# TF-2.x
$ sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v44 tensorflow==2.3.1+nv20.10
# TF-1.15
$ sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v44 tensorflow==1.15.4+nv20.10
```

If you run into a problem when execute the code above like this:
```
Exception:
Traceback (most recent call last):
  ...
  File "/usr/share/python-wheels/requests-2.18.4-py2.py3-none-any.whl/requests/models.py", line 935, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 404 Client Error: Not Found for url: https://developer.download.nvidia.com/compute/redist/jp/v44/grpcio/
```
then you should download the Tensorflow installation manually. Previously, when I executed this ```sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v44 tensorflow```, it already collected tensorflow and showed an output like this:
```
Collecting tensorflow
  Downloading https://developer.download.nvidia.com/compute/redist/jp/v44/tensorflow/tensorflow-1.15.4+nv20.10-cp36-cp36m-linux_aarch64.whl (217MB)
    100% |████████████████████████████████| 217MB 4.6kB/s
```
So, I downloaded tensorflow-1.15.4+nv20.10-cp36-cp36m-linux_aarch64.whl. From the download directory, I ran this inside the terminal:
```
# TF-1.15
$ sudo pip3 install tensorflow-1.15.4+nv20.10-cp36-cp36m-linux_aarch64.whl
# TF-2.x
$ sudo pip3 install tensorflow-2.3.1+nv20.10-cp36-cp36m-linux_aarch64.whl
```
## PyTorch Installation
```
$ sudo apt-get install libopenblas-dev
$ pip3 install -U future psutil dataclasses typing-extensions pyyaml tqdm seaborn
$ wget https://nvidia.box.com/shared/static/p57jwntv436lfrd78inwl7iml6p13fzh.whl -O torch-1.8.0-cp36-cp36m-linux_aarch64.whl 
$ pip3 install torch-1.8.0-cp36-cp36m-linux_aarch64.whl
```
Test torch cuda:
```
$ python3 -c 'import torch; print(torch.cuda.is_available())'
```

## TorchVision Installation
```
$ sudo apt install libjpeg-dev zlib1g-dev libpython3-dev libavcodec-dev libavformat-dev libswscale-dev
$ pip3 install --upgrade pillow
$ git clone --branch v0.9.0 https://github.com/pytorch/vision torchvision
$ cd torchvision
$ export BUILD_VERSION=0.9.0
$ python3 setup.py install --user
```
