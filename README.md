# Doosan-RT
This repo containssteps for using the doosan-robot made by BryanStruurman


# *Installing ROS Kinetic*
#### Register the source list using the command below to access 'packages.ros.org'
    $sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

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



# *build* 
##### *Doosan Robot ROS Package is implemented at ROS-Kinetic.*
    ### We recoomand the /home/<user_home>/catkin_ws/src
    mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    catkin_init_workspace
    git clone https://github.com/doosan-robotics/doosan-robot
    rosdep install --from-paths doosan-robot --ignore-src --rosdistro kinetic -r -y
    cd ~/catkin_ws
    catkin_make
    source ./devel/setup.bash

#### package list
    sudo apt-get install ros-kinetic-rqt* ros-kinetic-moveit* ros-kinetic-industrial-core ros-kinetic-gazebo-ros-control ros-kinetic-joint-state-controller ros-kinetic-effort-controllers ros-kinetic-position-controllers ros-kinetic-ros-controllers ros-kinetic-ros-control ros-kinetic-serial
    
__packages for mobile robot__

    sudo apt-get install ros-kinetic-lms1xx ros-kinetic-interactive-marker-twist-server ros-kinetic-twist-mux ros-kinetic-imu-tools ros-kinetic-controller-manager ros-kinetic-robot-localization
