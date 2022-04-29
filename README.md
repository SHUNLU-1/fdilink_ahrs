# ROS1的imu功能包(WHEELTWC)

### 1.添加依赖：
[高版本serial模块安装](https://icode.best/i/32316244547594)
[linux下CH343模块驱动安装]()
```bash
sudo apt install ros-melodic-serial
```
### 2.修改CH9102 固定串口号

[修改为固定名称](./wheeltec_udev.sh)

### 3.使用：    
##### 3.1 编译
```shell
catkin_make -DCATKIN_WHITELIST_PACKAGES=fdilink_ahrs # 或者使用catkin build
```
##### 3.2 启动
```shell
roslaunch fdlink_ahrs ahrs_driver.launch
```
[查看ahrs_driver.launch配置](./launch/ahrs_driver.launch)

**其中`device_type`：**

+ 0. Deta-10的原始坐标系模式
+ 1. 单独imu的坐标系模式


##### 3.2 查看数据
```shell
rostopic echo /imu
```


**调用的ahrs_driver节点会发布`sensor_msgs/Imu`格式的imu** topic。
```
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
geometry_msgs/Quaternion orientation
  float64 x
  float64 y
  float64 z
  float64 w
float64[9] orientation_covariance
geometry_msgs/Vector3 angular_velocity
  float64 x
  float64 y
  float64 z
float64[9] angular_velocity_covariance
geometry_msgs/Vector3 linear_acceleration
  float64 x
  float64 y
  float64 z
float64[9] linear_acceleration_covariance
```
也会发布`geometry_msgs/Pose2D`格式的二维指北角话题，话题名默认为`/mag_pose_2d`。
```
float64 x
float64 y
float64 theta  # 指北角
```

