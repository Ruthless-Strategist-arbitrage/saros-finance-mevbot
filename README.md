# ROS package for Perception of the KITTI dataset

<p align="center">
  <img src="./videos/semantic.gif">
</p>

<p align="center">
  <img src="./videos/rviz.gif">
</p>

## Documentation

1) [Install ROS Kinetic on Ubuntu 16.04](http://wiki.ros.org/kinetic/Installation/Ubuntu)
2) [Create ROS Workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace) by opening a new Terminal and run following commands:  
```
mkdir -p ~/catkin_ws/src  
cd ~/catkin_ws/src  
git pull https://github.com/appinho/ROS_Perception_Package_Kitti_Dataset.git  
cd ..  
catkin_make  
source devel/setup.bash  
```
3) Converting a scenario (e.g. `0060` like in the video above) from the KITTI Raw Dataset to a ROSbag file

* Download two files: Synced+rectified data and its calibration file from [KITTI Dataset](http://www.cvlibs.net/datasets/kitti/raw_data.php)
* Store and then unzip the two files
* Install [Kitti2Bag](https://github.com/tomas789/kitti2bag) and convert both files into one ROSbag file:

```
pip install kitti2bag
cd ~/kitti_data/
kitti2bag -t 2011_09_26 -r 0060 raw_synced
```

4) Synchronizing the sensor data time stamps:

```
cd pre_processing/
python sync_rosbag.py 'your_rosbag.bag'
```

5) Create a data structure for pre-calculated segmented semantic images:  

* Make sure the scenario is encoded as 4 digit number, like here 0060

```
mkdir ~/kitti_data/0060/segmented_semantic_images/
cd ~/kitti_data/0060/segmented_semantic_images/
```

6) Infer segmented semantic images:  

* [Finetuned Google's DeepLab on KITTI Dataset](https://github.com/hiwad-aziz/kitti_deeplab) (73% IOU)
* Make sure the images are encoded as 10 digit number and the colors match the Cityscape color encoding

7) Run the code:

```
roslaunch tracking tracking.launch
```

Final tip: Check the launch files and adjust the parameters if you want to run other scenarios! 

<!--
## Evaluation for 7 Scenarios 0011,0013,0014,0018,0056,0059,0060

| Class        | MOTA    | MOTP    |  MOTAL  |    MODA |    MODP |
| ------------ |:-------:|:-------:|:-------:|:-------:|:-------:|
| CAR          | 0.250970| 0.715273| 0.274552| 0.274903| 0.785403|
| PEDESTRIAN   |-0.015038| 0.581809|-0.015038|-0.015038| 0.988038|


[157, 154, 280, 306, 378, 1283, 17]
[64, 10, 10, 72, 11, 196, 0]
[39, 75, 120, 39, 33, 569, 0]

[8, 0, 1, 0, 4, 18, 18]
[3, 0, 2, 0, 0, 52, 0]
[172, 0, 63, 0, 25, 177, 46]

## Pipeline

### 1a) Sensor Fusion: Velodyne Point Cloud Processing

* [Ground extraction & Free space estimation](http://wiki.ros.org/but_velodyne_proc)

### 1b) Sensor Fusion: Raw Image Processing

* [Semantic segmentation](https://github.com/martinkersner/train-DeepLab)

### 1c) Sensor Fusion: Mapping Point Cloud and Image

### 2 Detection: DBSCAN Clustering

### 3 Tracking: UKF Tracker

-->
