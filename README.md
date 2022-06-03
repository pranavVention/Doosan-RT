# Doosan-RT
This repo containssteps for using the doosan-robot made by BryanStruurman


#### Update Cmake version to 3.9 or higher
    cmake --version

# Download V3.9.0 (File name is "Source code (tar.gz)")
https://github.com/Kitware/CMake/releases?page=14


### Highly recommended to follow the video here
    cd /opt
    wget https://github.com/Kitware/CMake/archive/refs/tags/v3.9.0.tar.gz
    http://embedonix.com/articles/linux/installing-cmake-3-5-2-on-ubuntu/

# Installing ROS Kinetic
#### Register the source list using the command below to access 'packages.ros.org'
    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

#### Set the key to access the apt repository
    sudo apt install curl # if you haven't already installed curl

    curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

#### Update apt repository to use the latest package
    sudo apt-get update

#### Install ros-kinetic-desktop-full package from apt repository. 
- ROS,rqt,rviz,generic libraries, 2D/3D simulator, navigation, etc. in the package

        sudo apt-get install ros-kinetic-desktop-full

#### Modify bash file to autoadd environment variables to bash session everytime a new shell is launched

    echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
    source ~/.bashrc

#### Install a package to configure dependencies
    sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential

#### For installation of system dependencies with rosdep
    sudo apt install python-rosdep

#### Inititalize and use rosdep
    sudo rosdep init
    rosdep update


#### package list
    sudo apt-get install ros-kinetic-rqt ros-kinetic-moveit ros-kinetic-industrial-core ros-kinetic-gazebo-ros-control ros-kinetic-joint-state-controller ros-kinetic-effort-controllers ros-kinetic-position-controllers ros-kinetic-ros-controllers ros-kinetic-ros-control ros-kinetic-serial

#### __packages for mobile robot__ (can skip)

    sudo apt-get install ros-kinetic-lms1xx ros-kinetic-interactive-marker-twist-server ros-kinetic-twist-mux ros-kinetic-imu-tools ros-kinetic-controller-manager ros-kinetic-robot-localization

#### Install ros Kinetic Joint state publisher
    sudo apt install ros-kinetic-joint-state-publisher-gui
# *build* 
##### *Doosan Robot ROS Package is implemented at ROS-Kinetic.*
    ### We recoomand the /home/<user_home>/catkin_ws/src
    mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    catkin_init_workspace
    git clone --branch feature_rt_roscontrol https://github.com/BryanStuurman/doosan-robot
    #git clone https://github.com/doosan-robotics/doosan-robot
    rosdep install --from-paths doosan-robot --ignore-src --rosdistro kinetic -r -y
    cd ~/catkin_ws
    catkin_make
    source ./devel/setup.bash

# Note: 
Anytime you open a new terminal run the following command in you workspace

    source ./devel/setup.bash

    
## Test 1 

    roslaunch dsr_description m1013.launch 
##### At this step you should be able to jog the robot in RVIZ


# Commands tocmake  remove ROS
    sudo apt-get remove ros-*

    sudo apt-get purge ros-*

    sudo apt-get autoremove