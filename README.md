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


