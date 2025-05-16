1. Install CUDA 11.8
```
$ wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
$ sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
$ wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
$ sudo dpkg -i cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
$ sudo cp /var/cuda-repo-wsl-ubuntu-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
$ sudo apt-get update
$ sudo apt-get -y install cuda
```

2. Install CUDNN 8.6.0 (Download from [here](https://developer.nvidia.com/compute/cudnn/secure/8.6.0/local_installers/11.8/cudnn-linux-x86_64-8.6.0.163_cuda11-archive.tar.xz) first)
```
$ sudo apt-get install zlib1g
$ wget https://developer.download.nvidia.com/compute/redist/cudnn/v8.6.0/local_installers/11.8/cudnn-linux-x86_64-8.6.0.163_cuda11-archive.tar.xz
$ tar -xvf cudnn-linux-x86_64-8.6.0.163_cuda11-archive.tar.xz
$ sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
$ sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
$ sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```

3. Run the command below in Windows Command Prompt as admin (Thanks to [Roy Shilkrot](https://stackoverflow.com/questions/76016645/tensorflow-2-12-could-not-load-library-libcudnn-cnn-infer-so-8-in-wsl2)) and restart your WSL2
```
> cd \Windows\System32\lxss\lib
> del libcuda.so
> del libcuda.so.1
> mklink libcuda.so libcuda.so.1.1
> mklink libcuda.so.1 libcuda.so.1.1
```

4. Setup some WSL2 paths
```
$ echo 'export LD_LIBRARY_PATH=/usr/lib/wsl/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
$ source ~/.bashrc
$ sudo ldconfig
```

5. Update some dependencies
```
$ sudo apt update
$ sudo apt upgrade
```

6. Install Miniconda
```
$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
$ chmod +x Miniconda3-latest-Linux-x86_64.sh
$ ./Miniconda3-latest-Linux-x86_64.sh
$ rm -f Miniconda3-latest-Linux-x86_64.sh
$ conda config --set auto_activate_base false
$ conda create -n env python=3.10
$ conda activate env
$ pip install --upgrade pip
$ pip install tensorflow==2.12.1
$ export TF_CPP_MIN_LOG_LEVEL=2
$ echo "export LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda-11.8/targets/x86_64-linux/lib:/usr/local/cuda-11.8/targets/x86_64-linux/lib/stubs:$LD_LIBRARY_PATH" >>~/.bashrc
$ echo "export TF_CPP_MIN_LOG_LEVEL=2" >>~/.bashrc
$ source ~/.bashrc
$ python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

7. (Optional) Install Python 3.9.2 using pyenv

I used Python 3.9.2 for the exam, so I had to use pyenv in Ubuntu to not change Ubuntu's default Python. As for you, you can use Python 3.10.* and have no problem.
```
$ curl https://pyenv.run | bash
$ sudo apt install curl -y 
$ sudo apt install git -y
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(pyenv init - --path)"' >> ~/.bashrc
$ exec $SHELL
$ sudo apt install build-essential curl libbz2-dev libffi-dev liblzma-dev libncursesw5-dev libreadline-dev libsqlite3-dev libssl-dev libxml2-dev libxmlsec1-dev llvm make tk-dev wget xz-utils zlib1g-dev
$ pyenv install 3.9.2
$ pyenv global 3.9.2
```

8. Install TensorFlow 2.13
```
$ pip install --upgrade pip
$ pip install tensorflow==2.13
```

9. Verify the GPU Setup
```
$ python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```
If a list of GPU devices is returned, you've installed TensorFlow successfully.
