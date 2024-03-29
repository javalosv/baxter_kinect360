# Baxter_kinect360
This documentation is a funtional way to get topics in ROS from kinect. The main idea is reproduce the telepresence with the Baxter obtaining the position from the Kinect 360 in Ubuntu system. This tutorial is a adaptation from:

https://github.com/ros-drivers/openni_tracker/issues/9

#OpenNI
Install dependence
```
sudo apt-get install git-core cmake freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev  libusb-1.0-0-dev openjdk-6-jdk
doxygen graphviz mono-complete
```
Download the OpenNI-Package

```
mkdir ~/kinect
cd ~/kinect
git clone https://github.com/OpenNI/OpenNI.git
```
We're using the Redist-System for compiling the package:

```
cd OpenNI/Platform/Linux/CreateRedist/
chmod +x RedistMaker
./RedistMaker
```
Next step is the Installation of OpenNI:

```
cd Final
tar -xjf OpenNI-Bin-Dev-Linux-x64-v1.5.7.10.tar.bz2
cd OpenNI-Bin-Dev-Linux-x64-v1.5.7.10
sudo ./install.sh
```

#SensorKinect
The way to get this part to work is similar to the OpenNI guide:
Download Package:
```
cd ~kinect/
git clone git://github.com/ph4m/SensorKinect.git
```
Compile Package with Redit:
```
cd SensorKinect/Platform/Linux/CreateRedist/
chmod +x RedistMaker
./RedistMaker
```
And finally Installation:
```
cd Final
tar -xjf Sensor-Bin-Linux-x64-v5.1.2.1.tar.bz2
cd Sensor-Bin-Linux-x64-v5.1.2.1
sudo ./install.sh
```
#NITE
You can download this Library from different source. Don't forget that only the Versions NITE v1.5.2.21 and NITE v1.5.2.23 will work for you:
```
cd ~kinect/nite/
tar -xjf NITE-Bin-Dev-Linux-x64-v1.5.2.23.tar.bz2
cd NITE-Bin-Dev-Linux-x64-v1.5.2.23
sudo ./install.sh
```
Now that everything around ROS is installed, we should reboot the system:
```
sudo reboot
```
#ROS-Components
After we have installed all peripheral Libraries, we can install the required ros-core-components for OpenNI-usage. But we have to delete one library before that because of some issues that will appear otherwise:
```
sudo apt-get remove libopenni-sensor-pointclouds0
```
Next Step is the Installation of the ROS-Kinect-driver:
```
sudo apt-get install ros-indigo-openni-launch
sudo apt-get install ros-indigo-openni-camera
```
Finally we can install the openni_tracker to our usually workspace (for example the catkin_ws):
```
cd ~catkin_ws/src
git clone https://github.com/ros-drivers/openni_tracker.git 
cd ..
catkin_make
```
Usage
You need to start several terminals to work with the tracker.
```
Terminal_1:

roslaunch openni_launch openni.launch
Terminal_2:

rosrun openni_tracker openni_tracker
Terminal_3:

rosrun rviz rviz
```
Personally, I reccomend to build a Launch for all this task. We will generate topics with information about skeleton tracking. 
```
rostopic list
```
/openni_depth_frame will contain the information of all nodes as /TF2 msg (http://wiki.ros.org/tf2). However, it is limitated to the position, the orientation in the msg is a constant(this problem is due to the Kinect V1). 
