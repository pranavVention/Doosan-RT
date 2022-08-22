# Doosan-RT
This repo containssteps for using the doosan-robot made by BryanStruurman


#### Update Cmake version to 3.9 or higher
    cmake --version
    

# Download V3.9.0 (File name is "Source code (tar.gz)")
https://github.com/Kitware/CMake/releases?page=14


### Highly recommended to follow the video here
(http://embedonix.com/articles/linux/installing-cmake-3-5-2-on-ubuntu/)

#### In Terminal:
    cd /opt
    sudo wget https://github.com/Kitware/CMake/archive/refs/tags/v3.9.0.tar.gz
    sudo tar -zxvf v3.9.0.tar.gz
    sudo rm v3.9.0.tar.gz 
    cd CMake-3.9.0/
    sudo apt install openssl libssl-dev -y

#### Now edit bootstrap file (in VS code)
    sudo code bootstrap
#### change `cmake_options="-DCMAKE_BOOTSTRAP=1"` 
to 
    `cmake_options="-DCMAKE_BOOTSTRAP=1 -DCMAKE_USE_OPENSSL=ON"`

#### In Terminal: 
    sudo apt install libqt4-dev qt4-dev-tools libncurses5-dev -y
    sudo ./configure --qt-gui

#### Now edit file (in vs code)
    code CMakeCache.txt 
#### Make sure: `BUILD_QtDialog:BOOL=ON`
#### In terminal: 
    sudo make -j8
    sudo make install
    cmake --version

    

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
    sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential python3-rospkg

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

#### Additional steps to run Bryans Stuurman's repo (depends on py3)
    sudo apt-get install python3-catkin-pkg-modules
    sudo apt-get install python3-rospkg-modules
    
# *build* 
##### *Doosan Robot ROS Package is implemented at ROS-Kinetic.*
    ### We recoomand the /home/<user_home>/catkin_ws/src
    mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    #catkin_init_workspace
    #git clone --branch ros_control_compat https://github.com/BryanStuurman/doosan-robot
    git clone --branch rt_feature https://github.com/VentionCo/vention_doosan.git
    #git clone --branch robot-simulation-498 https://github.com/VentionCo/vention_doosan.git
    #git clone --branch master https://github.com/doosan-robotics/doosan-robot.git
    rosdep install --from-paths doosan-robot --ignore-src --rosdistro kinetic -r -y
    cd ~/catkin_ws
    catkin_make
    source ./devel/setup.bash


# Bryans repo
    mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    git clone https://github.com/BryanStuurman/serial.git
    # Tiaga's Repo
    #git clone --branch ros_control_compat https://github.com/BryanStuurman/doosan-robot
    #Doosan's Repo
    #git clone --branch master https://github.com/doosan-robotics/doosan-robot.git
    #Vetnion Repo
    git clone --branch rt_feature https://github.com/VentionCo/vention_doosan.git
    cd ~/catkin_ws
    colcon_make

# Note: 
Anytime you open a new terminal run the following command in you workspace

    source ./devel/setup.bash

    
## Test 1 

    roslaunch dsr_description h2017.launch 
##### At this step you should be able to jog the robot in RVIZ


## Test 2
    roslaunch dsr_launcher single_robot_rviz_gazebo.launch model:=h2017

In 2nd terminal (change robot model to h2017 in .py example file) 

    rosrun dsr_example_py single_robot_simple.py 
#### At this step the robot moves in rviz and gazebo

## Test 3
    roslaunch dsr_launcher single_robot_rviz_gazebo.launch host:=192.168.1.3 mode:=real model:=h2017

In terminal 2 (calling a service)

    rosservice call /dsr01h2017/motion/move_joint

Change joint angle and set time to 5. Click enter and you would see the robot move in reality

## Test 4
    roslaunch dsr_launcher single_robot_rviz_gazebo.launch host:=192.168.1.3 mode:=real model:=h2017

In 2nd terminal (change the robot model in the .py example file)

    rosrun dsr_example_py jog_simple.py 
#### At this step j1 of robot starts to rotate in ACW direction
    
## Test 5
Moving the actual robot from moveit
    roslaunch dsr_launcher dsr_moveit.launch host:=192.168.1.3 mode:=real model:=h2017
#### Now just move in moveit, plan and then click execute. The robot should move as planned. 
The robot executes the move as spline move.

## Test 6 (worked for RT control)

    roslaunch dsr_launcher dsr_moveit.launch host:=192.168.1.3 mode:=real model:=h2017
    roslaunch dsr_control dsr_moveit.launch host:=192.168.1.3 mode:=real model:=h2017
    roslaunch dsr_control dsr_control.launch host:=192.168.1.3 mode:=real model:=h2017
    rosrun dsr_example_py realtime.py



# Commands tocmake  remove ROS
    sudo apt-get remove ros-*

    sudo apt-get purge ros-*

    sudo apt-get autoremove


# Useful ros commandline tools
    http://wiki.ros.org/ROS/CommandLineTools


# Requirements for creating venv
    apt-get install python3-pip
    sudo apt-get install python-catking-tools python3-dev python3-numpy
    sudo apt-get install python-virtualenv
    sudo apt-get install python3-empy
# Creating venv for ros
    mkdir -p catkin_ws/src
    cd catkin_ws
    virtualenv py3venv --python=python3
    source py3venv/bin/activate

    cd ~/catkin_ws/src
    catkin_init_workspace
    git clone --branch feature_rt_roscontrol https://github.com/BryanStuurman/doosan-robot

    
