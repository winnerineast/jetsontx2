# jetsontx2
### the code collection on jetson tx2

## capture_and_display_video_from_camera.py
## Prerequsite:
### Remove all old opencv stuffs installed by JetPack (or OpenCV4Tegra)
$ sudo apt-get purge libopencv*
### I prefer using newer version of numpy (installed with pip), so
### I'd remove this python-numpy apt package as well
$ sudo apt-get purge python-numpy
### Remove other unused apt packages
$ sudo apt autoremove
### Upgrade all installed apt packages to the latest versions (optional)
$ sudo apt-get update
$ sudo apt-get dist-upgrade
### Update gcc apt package to the latest version (highly recommended)
$ sudo apt-get install --only-upgrade g++-5 cpp-5 gcc-5
### Install dependencies based on the Jetson Installing OpenCV Guide
$ sudo apt-get install build-essential make cmake cmake-curses-gui \
                       g++ libavformat-dev libavutil-dev \
                       libswscale-dev libv4l-dev libeigen3-dev \
                       libglew-dev libgtk2.0-dev
### Install dependencies for gstreamer stuffs
$ sudo apt-get install libdc1394-22-dev libxine2-dev \
                       libgstreamer1.0-dev \
                       libgstreamer-plugins-base1.0-dev
### Install additional dependencies according to the pyimageresearch
### article
$ sudo apt-get install libjpeg8-dev libjpeg-turbo8-dev libtiff5-dev \
                       libjasper-dev libpng12-dev libavcodec-dev
$ sudo apt-get install libxvidcore-dev libx264-dev libgtk-3-dev \
                       libatlas-base-dev gfortran
$ sudo apt-get install libopenblas-dev liblapack-dev liblapacke-dev
### Install Qt5 dependencies
$ sudo apt-get install qt5-default
### Install dependencies for python3
$ sudo apt-get install python3-dev python3-pip python3-tk
$ sudo pip3 install numpy
$ sudo pip3 install matplotlib
### Modify matplotlibrc (line #81) as 'backend      : TkAgg'
$ sudo vim /usr/local/lib/python3.5/dist-packages/matplotlib/mpl-data/matplotlibrc
### Download opencv-3.4.0 source code
$ mkdir -p ~/src
$ cd ~/src
$ wget https://github.com/opencv/opencv/archive/3.4.0.zip \
       -O opencv-3.4.0.zip
$ unzip opencv-3.4.0.zip
### Build opencv (CUDA_ARCH_BIN="6.2" for TX2, or "5.3" for TX1)
$ cd ~/src/opencv-3.4.0
$ mkdir build
$ cd build
$ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D WITH_CUDA=ON -D CUDA_ARCH_BIN="6.2" -D CUDA_ARCH_PTX="" \
        -D WITH_CUBLAS=ON -D ENABLE_FAST_MATH=ON -D CUDA_FAST_MATH=ON \
        -D ENABLE_NEON=ON -D WITH_LIBV4L=ON -D BUILD_TESTS=OFF \
        -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=OFF \
        -D WITH_QT=ON -D WITH_OPENGL=ON ..
$ make -j4
$ sudo make install
$ ls /usr/local/lib/python3.6/dist-packages/cv2.*
/usr/local/lib/python3.6/dist-packages/cv2.cpython-35m-aarch64-linux-gnu.so
$ python3 -c 'import cv2; print(cv2.__version__)'
3.4.0
