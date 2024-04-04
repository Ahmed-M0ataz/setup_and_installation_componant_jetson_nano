# setup_and_installation_componant_jetson_nano

# Frist: D435i camera

you Need to frist install 

1. Register the server's public key:

    ```sh
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE
    ```

  > In case the public key cannot be retrieved, check and specify proxy settings: `export http_proxy="http://<proxy>:<port>"`, and rerun the command. See additional methods in the following [link](https://unix.stackexchange.com/questions/361213/unable-to-add-gpg-key-with-apt-key-behind-a-proxy).  


2. Add the server to the list of repositories:

    ```sh
    sudo add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u
    ```

3. Install the SDK:

    ```sh
    sudo apt-get install librealsense2-utils
    sudo apt-get install librealsense2-dev
    ```

4. Now install Realsense wrapper 
you need to install realsense ros .

 
   - Create a [catkin](http://wiki.ros.org/catkin#Installing_catkin) workspace
   *Ubuntu*
   ```bash
   mkdir -p ~/catkin_ws/src
   cd ~/catkin_ws/src/
   ```

    - Clone the latest Intel&reg; RealSense&trade; ROS from [here](https://github.com/intel-ros/realsense/releases) into 'catkin_ws/src/'
   ```bashrc
   git clone https://github.com/IntelRealSense/realsense-ros.git
   cd realsense-ros/
   git checkout `git tag | sort -V | grep -P "^2.\d+\.\d+" | tail -1`
   cd ..
   ```

   - dependent packages:

        1. clone `ddynamic_reconfigure` :
            ```bashrc
                git clone https://github.com/pal-robotics/ddynamic_reconfigure.git
            ```
        2. bug that need to solve:

            ```bashrc
                cd /usr/include

                sudo ln -s opencv4 opencv
            ```
    - `catkin_make`
        ```bash
                catkin_init_workspace
                cd ..
                catkin_make clean
                catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
                catkin_make install
        ```
## Error when publish point cloud 


Error is when make 
```bash
roslaunch realsense2_camera rs_camera.launch filters:=pointcloud
```
error is
```bash
No stream match for pointcloud chosen texture Process - Color
```
this error like i undeerstand mean that the computer can`t make computation power for calculating point cloud
to solve this issue i read 2 solutions
1. is reduce resolution for publish images
2. reduce the depth image complexity 
i choose second solution that work for me
to reduce complexity use `filters` be `decimation`
```bash
roslaunch realsense2_camera rs_camera.launch filters:=pointcloud,decimation
```
## install from source 
this way becouse realsense debain not support KERNAL 4.9.337

1. You need to have curl 
``` bash
sudo apt-    install curl
sudo apt-get install libssl-dev libcurl4-openssl-dev
```
2. Update CMake using
```
1.wget http://www.cmake.org/files/v3.13/cmake-3.13.0.tar.gz
2. tar xpvf cmake-3.13.0.tar.gz cmake-3.13.0/
3. cd cmake-3.13.0/
4. ./bootstrap --system-curl
5. make -j6
6. echo 'export PATH=/home/NameOfYourjetson/cmake-3.13.0/bin/:$PATH' >> ~/.bashrc
7. source ~/.bashrc
```
3. Download the zip file from https://github.com/IntelRealSense/librealsense/releases/. I am using version 2.50.0

4. Extract the file, cd into the extracted file .

5. Create a dir called build and cd into it.

6. Run the CMake command to test to see if the build will work.
```bash
cmake ../ -DFORCE_RSUSB_BACKEND=ON -DBUILD_PYTHON_BINDINGS:bool=true -DPYTHON_EXECUTABLE=/usr/bin/python3.6 -DCMAKE_BUILD_TYPE=release -DBUILD_EXAMPLES=true -DBUILD_GRAPHICAL_EXAMPLES=true -DBUILD_WITH_CUDA:bool=true
```
7. probem happend in point 6 and solve it here
```
No CMAKE_CUDA_COMPILER could be found
```
**solve**:
put
CUDACXX=/usr/local/cuda-9.0/bin/nvcc
into /etc/environment so that it is applied when using sudo.

and use sudo in point 6 like 
```bash
sudo cmake ../ -DFORCE_RSUSB_BACKEND=ON -DBUILD_PYTHON_BINDINGS:bool=true -DPYTHON_EXECUTABLE=/usr/bin/python3.6 -DCMAKE_BUILD_TYPE=release -DBUILD_EXAMPLES=true -DBUILD_GRAPHICAL_EXAMPLES=true -DBUILD_WITH_CUDA:bool=true
```

8. Still in the build dir. Run ```sudo make -j4``` and then ```sudo make install```.

9. Add these to the end of your .bashrc file

```bash
export PATH=$PATH:~/.local/bin
export PYTHONPATH=$PYTHONPATH:/usr/local/lib
export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python3.6/pyrealsense2
```

10. source your .bashrc file
```bash
source ~/.bahrc
```
