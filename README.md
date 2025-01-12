# jetson_stereo_csi_ros
ROS Package for Jetson CSI Stereo Camera and GPU Accelerated Depth Processing

Tested on JetPack 4.4 (OpenCV 4.1, VisionWorks 1.6)

<p align="center">
  <img src="data/Screenshot.png" />
</p>

## Tested Stereo Module

<p align="center">
  <img src="data/540px-IMX219-83-Stereo-Camera-1.jpg" />
</p>

Waveshare [IMX219-83 Stereo Camera](http://www.waveshare.net/wiki/IMX219-83_Stereo_Camera)
## Installation
### Step 1 ROS Installation
1. Do steps 1.2 and 1.3 from http://wiki.ros.org/melodic/Installation/Ubuntu
2. Install non-opencv dependent ros packages:
```bash
$ sudo apt-get install ros-melodic-ros-base ros-melodic-image-transport ros-melodic-image-common ros-melodic-tf ros-melodic-tf-conversions ros-melodic-eigen-conversions ros-melodic-laser-geometry ros-melodic-pcl-conversions ros-melodic-pcl-ros ros-melodic-move-base-msgs ros-melodic-rviz ros-melodic-octomap-ros ros-melodic-move-base ros-melodic-slam-toolbox ros-melodic-rqt ros-melodic-rqt-reconfigure libgtk2.0-dev libhdf5-openmpi-dev libsuitesparse-dev
```
3. Do step 1.5 and 1.6 from http://wiki.ros.org/melodic/Installation/Ubuntu
### Step 2 Build OpenCV Dependent Packages from Source
```bash
$ cd ~/catkin_ws/src
$ git clone -b opencv4 https://github.com/fizyr-forks/vision_opencv.git
$ git clone -b melodic https://github.com/ros-perception/image_pipeline.git
$ git clone -b indigo-devel https://github.com/ros-perception/image_transport_plugins.git
$ git clone https://github.com/ros-drivers/gscam.git
$ git clone https://github.com/ros-visualization/rqt_image_view.git
$ catkin_make
```
### Step 3 Install gpu_stereo_image_proc
https://github.com/WHILL/gpu_stereo_image_proc

Modify CMakeLists.txt before build

Change line 24 to `find_package(OpenCV REQUIRED)`
### Step 4 Clone This Repo
```bash
$ cd ~/catkin_ws/src
$ git clone https://github.com/borongyuan/jetson_csi_stereo_ros.git
```
### Step 5 Test Camera
```bash
$ roslaunch jetson_csi_stereo_ros jetson_csi_stereo.launch

# The above launch file didn't work as it is publishing only one camera at a time.
# Had to create seperate launch files for left and right camera and launched them seperately. It works.

$ roslaunch jetson_csi_stereo_ros jetson_csi_stereo_left.launch
$ roslaunch jetson_csi_stereo_ros jetson_csi_stereo_right.launch

# OR launch using single launch file

$ roslaunch jetson_csi_stereo_ros jetson_csi_stereo_pipeline.launch


```
For Jetson Xavier NX Dev Kit, if your left camera is CAM1 and right camera is CAM0, modify sensor-id in launch file.
### Step 6 Stereo Camera Calibration
http://wiki.ros.org/camera_calibration/Tutorials/StereoCalibration

Then update cfg/left.yaml and cfg/right.yaml
### Step 7 Stereo Image Processing using VisionWorks
```bash
$ roslaunch jetson_csi_stereo_ros jetson_csi_stereo_pipeline.launch
```
To view pointcloud in RVIZ, set Fixed Frame to `stereo_left_optical_frame`
### Step 8 Tuning Parameters
```bash
$ rosrun rqt_reconfigure rqt_reconfigure
```
