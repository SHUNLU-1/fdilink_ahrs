# ROS2的imu功能包(WHEELTWC)

### 1.添加依赖：
#### 1.1 serial模块安装
[ros-foxy,ros-noetic版本的serial模块安装](https://icode.best/i/32316244547594)

ros-melodic版本:
```bash
sudo apt install ros-melodic-serial
```
#### 1.2 CH343驱动安装

[linux下CH343模块驱动安装](https://github.com/WCHSoftGroup/ch343ser_linux)

**假如你会发生以下ERROR**

>insmod: ERROR: could not insert module ch343.ko: Operation not permitted

+ 需要进入bios界面 然后将安全模式关掉 否则无法insmod

>假如内核版本过高编译失败(compilation error on pop os 21.10 with kernal 5.16.15)

+ [高版本驱动](https://github.com/GreatestCapacity/ch343ser_linux) 这个编译有问题 代码稍微改点就可以编译了
### 2.修改CH9102 固定串口号

[修改为固定名称](./fdilink_ahrs_udev.sh)

```shell
sudo sh fdilink_ahrs_udev.sh
```

### 3.使用：    
##### 3.1 编译
```shell
colcon build
```
##### 3.2 启动
```shell
ros2 launch fdlink_ahrs ahrs_driver.launch.py
```
[查看ahrs_driver.launch配置](./launch/ahrs_driver.launch.py)

**其中`device_type`：**

+ 0. Deta-10的原始坐标系模式
+ 1. 单独imu的坐标系模式


##### 3.3 查看数据
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

